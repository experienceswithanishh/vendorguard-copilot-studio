# VendorGuard — Dataverse Schema

## Table 1 — Contract (vg_contract)

| Display Name | Schema Name | Type | Required |
|---|---|---|---|
| Contract Name | vg_contractname | Single line of text | Yes |
| Contract Number | vg_contractnumber | Single line of text | Yes |
| Vendor Name | vg_vendorname | Single line of text | Yes |
| Source Email | vg_sourceemail | Single line of text | No |
| Contract Status | vg_contractstatus | Choice: Received, Under Review, Compliant, Non-Compliant, Escalated | Yes |
| Contract Value | vg_contractvalue | Currency | No |
| Contract Start Date | vg_contractstartdate | Date Only | No |
| Contract End Date | vg_contractenddate | Date Only | No |
| Vendor Country | vg_vendorcountry | Single line of text | No |
| Contract Summary | vg_contractsummary | Multiple lines of text | No |
| Overall Compliance Score | vg_overallcompliancescore | Choice: Red, Amber, Green | No |
| Assigned Reviewer | vg_assignedreviewer | Single line of text | No |
| Upload Date | vg_uploaddate | Date and Time | No |
| Contract PDF | vg_contractpdf | File | No |

## Table 2 — Vendor (vg_vendor)

| Display Name | Schema Name | Type | Required |
|---|---|---|---|
| Vendor Name | vg_vendorname | Single line of text | Yes |
| Vendor Email | vg_vendoremail | Single line of text | No |
| Vendor Country | vg_vendorcountry | Single line of text | No |
| Vendor Category | vg_vendorcategory | Choice: IT Services, Software, Logistics, Consulting, Hardware, Other | No |
| Risk Tier | vg_risktier | Choice: High, Medium, Low | No |
| Vendor Status | vg_vendorstatus | Choice: Active, Inactive, Under Review | No |
| Vendor Website | vg_vendorwebsite | Single line of text | No |
| Vendor Notes | vg_vendornotes | Multiple lines of text | No |

## Table 3 — Compliance Rule (vg_compliancerule)

| Display Name | Schema Name | Type | Required |
|---|---|---|---|
| Rule Name | vg_rulename | Single line of text | Yes |
| Compliance Dimension | vg_compliancedimension | Choice: Commercial, Legal, Data Privacy, SLA & Performance, Regulatory | Yes |
| Rule Description | vg_ruledescription | Multiple lines of text | Yes |
| What to Check | vg_whattocheck | Multiple lines of text | Yes |
| Severity | vg_severity | Choice: Must Have, Should Have, Nice to Have | Yes |
| Rule Status | vg_rulestatus | Choice: Active, Inactive | No |
| Example Clause | vg_exampleclause | Multiple lines of text | No |

## Table 4 — Compliance Result (vg_complianceresult)

| Display Name | Schema Name | Type | Required |
|---|---|---|---|
| Result Summary | vg_name | Single line of text | Yes |
| Score | vg_score | Choice: Red, Amber, Green | Yes |
| Reason | vg_reason | Multiple lines of text | Yes |
| Extracted Clause Text | vg_extractedclausetext | Multiple lines of text | No |
| Compliance Dimension | vg_compliancedimension | Choice: Commercial, Legal, Data Privacy, SLA & Performance, Regulatory | Yes |
| Recommendation | vg_recommendation | Multiple lines of text | No |
| Reviewed By | vg_reviewedby | Single line of text | No |
| Review Date | vg_reviewdate | Date and Time | No |
| Contract | vg_contract | Lookup → vg_contract | Yes |
| Compliance Rule | vg_compliancerule | Lookup → vg_compliancerule | No |

## Table 5 — Review Report (vg_reviewreport)

| Display Name | Schema Name | Type | Required |
|---|---|---|---|
| Report Title | vg_name | Single line of text | Yes |
| Report Status | vg_reportstatus | Choice: Draft, Sent for Review, Reviewed, Approved, Rejected | Yes |
| Report Summary | vg_reportsummary | Multiple lines of text | No |
| Overall Score | vg_overallscore | Choice: Red, Amber, Green | No |
| Assigned Reviewer | vg_assignedreviewer | Single line of text | No |
| Reviewer Email | vg_revieweremail | Single line of text | No |
| Date Generated | vg_dategenerated | Date and Time | No |
| Date Reviewed | vg_datereviewed | Date and Time | No |
| Red Flags Count | vg_redflagscount | Whole Number | No |
| Amber Flags Count | vg_amberflagscount | Whole Number | No |
| Green Flags Count | vg_greenflagscount | Whole Number | No |
| Reviewer Comments | vg_reviewercomments | Multiple lines of text | No |
| Contract | vg_contract | Lookup → vg_contract | Yes |
