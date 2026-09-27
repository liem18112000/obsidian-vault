---
title: "Testing statistics and remaining impediment after implementation."
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530459109/Testing+statistics+and+remaining+impediment+after+implementation.
space: "LUZ"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2021-08-04
attachments: 13
tags:
  - confluence
  - programming
  - space/luz
---

# Testing statistics and remaining impediment after implementation.

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-08-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530459109/Testing+statistics+and+remaining+impediment+after+implementation.)
> Relevance 0.731 · topic `programming`

## Structure

<span class="inline-comment-marker" ref="59ac15d7-26f6-49bd-b75e-ae4826b09a2b">The new structure of </span>**<span class="inline-comment-marker" ref="59ac15d7-26f6-49bd-b75e-ae4826b09a2b">luz_address_normalizer</span>**<span class="inline-comment-marker" ref="59ac15d7-26f6-49bd-b75e-ae4826b09a2b"> after implementation will have these table:</span>


![[20530459109-image2021-7-16_14-38-59.png]]



### Table **address_batch_request**:


![[20530459109-image2021-7-16_14-42-1.png]]



This table will receive the batch request from outsite (example: luz_tenant_dir) then after finish the normalize process, return normalized addresses.

Support up to 1k address for each batch now.

Field explaination:

- **data**: encrypted raw original addresses.
- **unnormalized_address_amount**: remaning addresses number need to do normalized, for example i send 1.000 addresses in for this batch, 900 is already existed in cache → this number is 100.
- **result**: mapping between raw address's uuid and hash key of this normalized address in **address_cache** table (format: **uuid1:hashkey1;uuid2:hashkey2**).
- **status**: WAIT_FOR_NORMALIZING, PROCESSING, FINISHED, ERROR
- **provider_running_batch**: point the the batch request send to eirene.
- **nomalized_data**: store data directly from ERENE

### Table **address_cache**:


![[20530459109-image2021-7-16_14-58-53.png]]



Save cache addresses return from Eirene.

Field explaination:

- **key**: hashed from raw address.
- **raw_address**: encrypted raw address.
- **normalized_address**: encrypted normalized address from Eirene.
- **cache_hit**: times this address is use
- **normalize_status**: FOUND, NOT_FOUND, REFRESHING, FAILED_REFRESH

### Table **provider_running_batch**:


![[20530459109-image2021-7-16_14-59-39.png]]



Send batch request to Eirene.

Field explaination:

- **batch_token**: token for check status from EIRENE of this batch (10k for now)
- **file_output_token**: use to get result from Eirene after finished.
- **status**: NORMALIZING, FINISHED, ERROR
- **extract_stastus** : PROCESSING, FINISHED

  

### Common relation between 3 tables:


![[20530459109-image2021-7-16_15-27-30.png]]



  

### Old implementation:


![[20530459109-image2021-7-16_15-40-8.png]]



### New implementation:


![[20530459109-image2021-7-16_15-46-54.png]]



## Testing statistics

1.  **Refresh cache flow**  
    **Case 1:** Do refresh when have no processing request sent to EREINE (send continuously request to EREINE with rule maximum 3 requests per time).  
    ~~Before: Took** ~30 minutes** for **30K addresses** ~~  
    After: **Took 14 mins mins for 30K addresses (happy case)**  
    

![[20530459109-refresh-70k-desc.PNG]]

  
      
    **Case 2:** Do refresh when have already 2 processing requests to EREINE. Then send 1 request then stop refreshing flow and continue by *auto trigger job everyday at midnight.*  
    

![[20530459109-max3-desc.png]]



**2. Main flow:**

~~Before: 20 minutes for extract 10k addresses from eirene.~~

After: **took 15s to extract 10K addresses**


![[20530459109-image2021-7-16_16-24-11.png]]



  

**3. Get result flow:**

~~Before: 1min for 1K addresses~~

After: **20 seconds for get 1k addresses.**

## Impediment

### Main flow take 20 mins for handle extract data from Eirene to address_cache table.

***For example with the old flow, what we can do with 20 mins:***

- A sender send matching-runs request that contain 60.000 recipients for matching.
- We send max 3 batches to eirene, in 10 min they all return finish, number of addresses we normalized is 30.000.
- In next 10 mins, we send next 3 batches and also receive results in 10 min (30.000 again)
- So in 20 mins we receive 60.000 addresses normalized.
- For example only this sender use the api, and it take next 10 min to to matching, then this matching-runs finish in 30 mins.

***But with the new approach we faces the impediment:***

after we receive 10k normalized addresses from Eirene, luz_address_normalizer take 20 mins to prepare data and complete insert into address_cache table.

the reason now is it take time for each of these phases 10 mins


![[20530459109-image2021-7-16_16-12-18.png]]



we already apply executor to make it can run with core-threads="10" for both phase.

Code for encrypt now:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5648fa74-7153-4ea6-9399-8f7d7cd98c07" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom">

<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
  private List<AddressCache> buildListAddressCacheFrom(Map<String, AddressForBatchNormalize> addressMap, List<AddressForBatchNormalize> part) {
        return part.stream()
                .map(addressForBatchNormalize -> {
                    AddressCache addressCache = AddressCache.builder().cacheTime(LocalDateTime.now(ZoneOffset.UTC))
                        .cacheHit(0)
                        .cacheTime(LocalDateTime.now(ZoneOffset.UTC))
                        .key(addressForBatchNormalize.getAddressTrackingId())
                        .encryptedRawAddress(encryptor.encrypt(JsonUtils.toJson(addressMap.get(addressForBatchNormalize.getAddressTrackingId()))))
                        .encryptedNormalizedAddress(encryptor.encrypt(JsonUtils.toJson(addressForBatchNormalize)))
                        .normalizeStatus(NormalizeStatusConverter.from(addressForBatchNormalize.getStatus()))
                        .build();
                LOGGER.log(Level.WARNING, "TEST PURPOSE: build AddressCache with key: " + addressForBatchNormalize.getAddressTrackingId());
                return addressCache;
                }).collect(Collectors.toList());
    }
```

</div>

</div>

  

  

### **Improvement that the time take for handle extract result data too long **

We add one more column on table **address_batch_request** that will store directly the result from ERENE so it will not need to wait for data to be stored in table **cache_result**.

So for the uncached address, we will follow these steps:

1.  Send batch normalized to Erene 
2.  Get results from Erene
3.  Build the address directly from results 


![[20530459109-image2021-8-1_14-9-49.png]]



1.  Store in cache in background =\> so no time affect to the main flow


![[20530459109-image2021-8-1_23-32-30.png]]



  

With the new approach, we just take around **15s** to extract **10K** address from Erene (the old way take 20 mins like mention above)

### Out of slot for sending batch request for refresh cache flow.

solution: consider using parallel API from Eirene.

  

### After enhancement:

**Caching flow on dev server:**

- Matching**101**recipients then takes**7 mins**(**case not yet cached - Matching ID test: 1456**) and**2 mins**(**case have cached address - Matching ID test: 1457**) 
- Matching**50K**recipients then takes**30** **mins**(**case not yet cached - Matching ID test: 1458**) and**8** **mins**(**case have cached address - Matching ID test: 1460**)

**Refresh cache on dev server:**

- **30K**records takes **(Cache ID test: 1004, Batch running ID test: 253, 254, 255)**
  - **14mins  (happy case)**
  - **54mins (worst case) because the cronjob to update data into address cache run every 10mins**
