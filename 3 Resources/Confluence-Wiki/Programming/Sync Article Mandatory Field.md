---
title: "Sync Article Mandatory Field"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38218817905/Sync+Article+Mandatory+Field
space: "Helios"
topic: programming
relevance: 0.773
depth: 3
updated: 2021-03-26
attachments: 0
tags:
  - confluence
  - programming
  - space/helios
---

# Sync Article Mandatory Field

> [!info] Imported from Confluence
> Space **Helios** · updated 2021-03-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38218817905/Sync+Article+Mandatory+Field)
> Relevance 0.773 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_38218817905_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-38491" macro-id="c8623069-7c24-4ec5-b917-e59258b70b3a" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-38491" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-38491</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

Relevant API

- Sync Article
- Sync Variant

  

**Article** Mandatory Field:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d33cf9ef-6c27-4607-ad76-edbed111a243" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://bitbucket.org/axonivy-prod/luz_pos/src/master/src/main/java/com/axonivy/pos/utils/ArticleUtils.java

public static boolean isInvalidArticle(ArticlePOS article) {
        return isInvalidGenaralValue(article) ||
                isInvalidName(article) || 
                isInvalidUnit(article);
        
    }
    
    private static boolean isInvalidName(ArticlePOS article) {
        return Strings.isNullOrEmpty(article.getNameDE()) &&
                Strings.isNullOrEmpty(article.getNameEN()) &&
                Strings.isNullOrEmpty(article.getNameIT()) &&
                Strings.isNullOrEmpty(article.getNameFR());
    }
    
    private static boolean isInvalidDescription(ArticlePOS article) {
        return Strings.isNullOrEmpty(article.getDescriptionDE()) &&
                Strings.isNullOrEmpty(article.getDescriptionEN()) &&
                Strings.isNullOrEmpty(article.getDescriptionIT()) &&
                Strings.isNullOrEmpty(article.getDescriptionFR());
    }
    
    private static boolean isInvalidUnit(ArticlePOS article) {
        return Strings.isNullOrEmpty(article.getUnitDE()) &&
                Strings.isNullOrEmpty(article.getUnitEN()) &&
                Strings.isNullOrEmpty(article.getUnitIT()) &&
                Strings.isNullOrEmpty(article.getUnitFR());
    }
    
    private static boolean isInvalidGenaralValue(ArticlePOS article) {
        return (Strings.isNullOrEmpty(article.getArticleNumber()) ||
                article.getAccountingTags()== null ||
                article.getStatus() == null ||
                article.getDefaultQuantity() == null ||
                article.getUpdateTime()== null);
    }
```

</div>

</div>

  

**Variant** Mandatory Field:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="865f1dd3-02f3-4039-b80b-8d1d7caa1a57" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
https://bitbucket.org/axonivy-prod/luz_pos/src/master/src/main/java/com/axonivy/pos/utils/VariantUtils.java
public static boolean isInvalid(VariantPOS variant) {
        return isInvalidVariantOptions(variant)
                || isInvalidGenaralValue(variant)
                || isInvalidName(variant);
    }

    private static boolean isInvalidVariantOptions(VariantPOS variant) {
        return variant.getVariantOptions() == null || variant.getVariantOptions().isEmpty();
    }

    private static boolean isInvalidName(VariantPOS variant) {
        return Strings.isNullOrEmpty(variant.getNameDE())
                && Strings.isNullOrEmpty(variant.getNameEN())
                && Strings.isNullOrEmpty(variant.getNameIT())
                && Strings.isNullOrEmpty(variant.getNameFR());
    }

    private static boolean isInvalidDescription(VariantPOS variant) {
        return Strings.isNullOrEmpty(variant.getDescDE())
                && Strings.isNullOrEmpty(variant.getDescEN())
                && Strings.isNullOrEmpty(variant.getDescIT())
                && Strings.isNullOrEmpty(variant.getDescFR());
    }

    private static boolean isInvalidGenaralValue(VariantPOS variant) {
        return variant.getArticleId() == null
                || (Strings.isNullOrEmpty(variant.getNumber())
                || Strings.isNullOrEmpty(variant.getAccountingTags())
                || variant.getDefaultQuantity() == null
                || variant.getUpdateDate() == null);
    }
```

</div>

</div>
