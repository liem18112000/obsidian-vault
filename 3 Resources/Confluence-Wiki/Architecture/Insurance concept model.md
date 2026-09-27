---
ai_hash: 1087bcc81c3c46b0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675759310/Insurance+concept+model
space: LUZCOMP
status: reference
tags:
- confluence
- architecture
- space/luzcomp
title: Insurance concept model
topic: architecture
type: source
updated: 2016-07-08
---

# Insurance concept model

> [!info] Imported from Confluence
> Space **LUZCOMP** · updated 2016-07-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZCOMP/pages/20675759310/Insurance+concept+model)
> Relevance 0.711 · topic `architecture`

![[20675759310-Insurance.png]]



1.  Rule for insurance_type is **UVG:**
    1.  Rate for men and for women in same group always equals

    2.  SIT_code in each group of UVG always = 5040

    3.  Defined by a character + a number (e.g. A1, T2, Z1) applied for: Insurance code string (after "/" of insurance allocation), and insurance_code_string for insurance_type is UVG

    4.  There are 4 possible of number in insurance allocation of UVG 0, 1, 2, 3

        1.  \- 0 = not insured  
            - 1 = Insured with a deduction on the payslip SIT = 5040  
            - 2 = Insured but paid by the company ( no deduction on the payslip)  
            - 3 = Special case similar to 2

        2.  

![[20675759310-image2016-6-10 16-50-20.png]]



    5.  Insurance allocation to the company (insurance_contract)
        1.  There are only three different codes available: A, B and Z
        2.  For every code allocated to the company we will a A1 and A2 or the equivalent.  
              
              
2.  Rule for insurance_type **UVGZ**:
    1.  <span style="line-height: 1.42857;">Rate for men and for women in same group always equals</span>
    2.  <span style="line-height: 1.42857;">Defined by a character + a number (e.g. A1, T2, Z1) applied for: Insurance code string (after "/" of insurance allocation), and insurance_code_string for insurance_type is UVGZ</span>
    3.  <span style="line-height: 1.42857;">There are 4 possible of number in insurance allocation of UVGZ 0, 1, 2, 3</span>
    4.  Insurance allocation the to company creates the following codes:<span style="line-height: 1.42857;">  
        </span>

<span style="line-height: 1.42857;">- 10 = not insured (not created)</span>

<span style="line-height: 1.42857;">- 11 = Insured till 148'200.00 (126'000.00) with a deduction on the payslip SIT = 5041</span>

<span style="line-height: 1.42857;"><span style="line-height: 1.42857;">- 12 = Insured over 148'200.00 (126'000.00) with a deduction on the payslip SIT = 5042</span>  
</span>  
  

1.  Rule for insurance_type **KTG**:
    1.  Rule for insurance_type is KTG:
    2.  Rate for men and for women in same group always equals
    3.  Defined by a character + a number (e.g. 11, 12) applied for: Insurance code string (after "/" of insurance allocation), and insurance_code_string for insurance_type is KTG
    4.  There are 3 possible of number in insurance allocation of KTG 0, 1, 2,
    5.  Insurance allocation the to company creates the following codes:

10 = not insured (not created)

<span style="line-height: 1.42857;">11 = Insured till 120'000.00 with a deduction on the payslip SIT = 5045</span>

12 = Insured over 120'000.00 with a deduction on the payslip SIT = 5046 (not supported yet)

%% ai-graph-start %%

**Related notes:**
- [[Documentation Implementation of Swissdec ELM 5.5 TariTemp (Single-Branch)]]
- [[AHV21 concept]]

%% ai-graph-end %%