---
ai_hash: 3c70cd5fd78f4ea7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.79
entities: []
relevance: 0.769
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47493939202/AHV21+concept
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: AHV21 concept
topic: architecture
type: source
updated: 2023-12-14
---

# AHV21 concept

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-12-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47493939202/AHV21+concept)
> Relevance 0.769 · topic `architecture`

<div class="toc-macro client-side-toc-macro non-printable conf-macro output-block" hasbody="false" headerelements="H1,H2" macro-id="69e53d5e-f61a-4189-acd8-762566ccfac3" macro-name="toc" numberedoutline="false" structure="list">

</div>

# Current definiton of AHV compulsary

## //Formula 9012 - AHV_NO_DUTY_AMOUNT

if (!variables.resolveVariableValue('AHV_DUTY')) { payslip.getSalaryItemValueByCode('9010'); }  
else if (variables.resolveVariableValue('AHV_DEDUCTION')) { def cumYearToDate9010 = payslips.cumYearToDate('9010', 'AHV_DEDUCTION', payslip.periodFrom);  
def cumYearlyValueToDateAHVAmount = contracts.cumYearlyValueToDate(variables.resolveVariableValue('AHV_FREE_AMOUNT'), 'AHV_DEDUCTION', payslip.periodFrom);  
def cumYearToPreviousMonth9012 = payslips.cumYearToPreviousMonth('9012', payslip.periodFrom);  
def result = (math.max(0, math.min(cumYearToDate9010, cumYearlyValueToDateAHVAmount)) - cumYearToPreviousMonth9012);  
return math.round(result, 0.05); } else 0.00;

## //AHV_DEDUCTION

(employee.ageOn(payslip.getReferencePayslipPeriodFrom()) \>= variables.resolveVariableValue('AHV_RETIREMENT_AGE'))

## //AHV_RETIREMENT_AGE

employee.isMale() ? 65 : 64

## //AHV_FREE_AMOUNT

16800

## //AHV_DUTY

(payslip.getReferencePayslipPeriodFrom().getYear() - employee.getBirthDay().getYear()) \>= variables.resolveVariableValue('AHV_MIN_AGE')

# Required changes

## Set the right retirement age for woman between 2025 and

### Change entry in variable AHV_RETIREMENT_AGE

for male it is still 65, for woman the following rule in correct YAML code has to be added:

if currentYear \< 2025 → 64

else if current Year 2025 → 64yrs and 3mts

else if currentYear = 2026 → 64yrs and 6mts

else if currentYear = 2027 → 64yrs and 9mts

else if currentYear \> 2027 → 65

### Ev. change in variable AHV_DEDUCTION

The result of the methode employee.**ageOn**(payslip.getReferencePayslipPeriodFrom() must be in minimum number of years and **months**

## Possibility to disable the free amount

### Adding checkbos to disable free amount in Employee.contract.temporal

- Field description:

  - de: Verzicht AHV-Freibetrag

  - en: Waiver AHV-Free amount

- no validation to set the flag (perhaps in future)

- warning if the flag is set

  - de: ACHTUNG! Bei Personen im Rentenalter wird bei aktivierter Checkbox der Freibetrag von jährlich CHF 16’800 nicht abgezogen.

  - en : ATTENTION: For persons of retirement age, the AHV-Free amount of CHF 16,800 per year is not deducted if the checkbox is activated.

### Changing global variable AHV_FREE_AMOUNT

disableFreeAmount.true ? 0 : 16800

# Global Variable Definition after implementation of AHV21 stories

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47493939202_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-107682" macro-id="488590fa-4d1a-49e9-a228-3804a610a82b" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-107682" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-107682</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

%% ai-graph-start %%

**Related notes:**
- [[Insurance concept model]]

%% ai-graph-end %%