# Cloud File Transfer (CFT) Pricing

!> CFT will implement billing through [TechBiz](https://www.developer.tech.gov.sg/products/categories/platform/techbiz/overview.html) starting **1 July 2025**. To continue using the service, all users—new and existing—must subscribe to CFT via TechBiz. Once subscribed through TechBiz, billing for CFT will be managed directly by TechBiz.

This page outlines the proposed and indicative CFT pricing rates to help you plan and budget accordingly. 

?> Please be informed that while our pricing methodology and structure have been substantially finalised, the figures presented are currently undergoing review and may be subject to modification. We value your patience and understanding during this process.

<small>If you are completely new to CFT and have onboarding questions, please contact us using [this form](https://form.gov.sg/665978af9040a6be24d3978b?665982626cae0284c464a22a=Cloud+File+Transfer).
</small>

##  CFT Pricing Overview

CFT's monthly charges are calculated at the **Project level** and based on two components:

- [Component 1: File Transfer Count](#component-1-file-transfer-count)
- [Component 2: Data Volume + Feature Usage](#component-2-data-volume-feature-usage)

### Component 1: File Transfer Count

You are billed based on the total number of files transferred, as uploaded from the **Sender Application**.

- Each successful file transfer is counted, including zipped folders.
- One zipped folder is considered one file for the purposes of file transfer billing — regardless of how many files are inside.

|      Tier     |      Files in Tier          |      Price per File (Proposed Rate in SGD)    |      Max Cost for Tier (in SGD)     |
|---------------|-----------------------------|-------------------------|----------------------------|
|     Tier 1    |     First 1,000 files       |     S$1.25               |     S$1,250.00              |
|     Tier 2    |     Next 9,000 files        |     S$0.25               |     S$2,250.00              |
|     Tier 3    |     Next 90,000 files       |     S$0.05               |     S$4,500.00              |
|     Tier 4    |     Beyond 100,000 files    |     S$0.01               |     Based on usage         |

**Example:** If your project processes 120,000 files in a month:

- Tier 1: 1,000 × $1.25 = S$1,250.00
- Tier 2: 9,000 × $0.25 = S$2,250.00
- Tier 3: 90,000 × $0.05 = S$4,500.00
- Tier 4: 20,000 × $0.01 = S$200.00

Summing it all up ➡ **Total File Transfer Charges = S$8,200.00**

### Component 2: Data Volume + Feature Usage

Charges are also applied based on the amount of data processed and the number of features used in each workflow.

CFT Priced Feature List:
- Scanner or Scanner-Bypass (mandatory)
- Content Disarm & Reconstruction (CDR)
- Encryption
- Decryption

|      Features Used     |      Price per GB (Proposed Rate in SGD)     |
|------------------------|-----------------------|
|     1 Feature          |     S$1.00             |
|     2 Features         |     S$1.50             |
|     3 Features         |     S$2.00             |
|     4 Features         |     S$2.50             |

**Important notes:**

- While each zipped folder is considered one file for file transfer count, data usage charges are calculated based on the total extracted size of the contents inside the zipped folder. For example, a 10MB zipped folder containing 100MB of extracted content will be charged based on 100MB.
- The same costs apply across all user environments, for example, Staging and Production environments.

## Cost calculation

Do take note of the following guidelines: 
- Charges are consolidated per Project per month.
- At least 1 feature (Scanner or Scanner-Bypass) is required per file.
- Data volume is grouped by number of features used, then **rounded up to the nearest GB** before pricing is applied.

### Example

Project Summary:
- 120,000 total file transfers (includes zipped folders)
- Workflows:
    - Workflow A: 28.2 GB extracted size, 1 Feature
    - Workflow B: 50.4 GB, 3 Features
    - Workflow C: 10.7 GB, 4 Features

Calculation:

1.	File Transfer Charges =  **S$8,200.00**
2.	Data Usage Charges (Rounded off):
    - 1 Feature: 29 GB × S$1.00 = S$29.00
    - 3 Features: 51 GB × S$2.00 = S$102.00
    - 4 Features: 11 GB × S$2.50 = S$27.50

    → Total Data Usage Charges = **S$158.50**

**➡ Total Monthly Bill = S$8,358.50**

### Additional notes

<!--  Files that are classified as Sensitive-High, password-protected and/or encrypted in formats that cannot be scanned by CFT must use the Scanner-Bypass feature.
-->
- To help with cost computation, use the [CFT cost calculator](https://go.gov.sg/cft-pricing-calculator). This file is hosted on WOG SharePoint. Please download a copy to use the file.
- Need to check past CFT usage? You can [raise an SR](https://go.gov.sg/cft-sm) to request project stats. 

- CFT pricing is applied only for successful file transfers and processed data, tracked monthly at the Project level.

## Indicative pricing

The rates defined in the previous sections are the proposed rates that are currently pending approval.

**Note:** The rates below are the indicative pricing rates that  represent the upper range of the potential approved cost. These provide a conservative estimate **for AOR and budgeting purposes**.

<u>File Transfer Count</u>

|      Tier     |      Files in Tier          |      Indicative Price (in SGD) |  
|---------------|-----------------------------|-------------------------|
|     Tier 1    |     First 1,000 files       |     S$1.25 - S$1.30       |
|     Tier 2    |     Next 9,000 files        |     S$0.25 - S$0.26       |   
|     Tier 3    |     Next 90,000 files       |     S$0.05 - S$0.06       |   
|     Tier 4    |     Beyond 100,000 files    |     S$0.01 - S$0.02       |   

<u>Data Volume + Feature Usage</u>

|      Features Used     |    Indicative Price per GB (in SGD)    |
|------------------------|-----------------------|
|     1 Feature          |     S$1.00 - S$1.05     |
|     2 Features         |     S$1.50 - S$1.60     |
|     3 Features         |     S$2.00 - S$2.10     |
|     4 Features         |     S$2.50 - S$2.60     |

## Billing Timeline

From 1 July 2025, CFT will officially start billing using the  [TechBiz](https://www.developer.tech.gov.sg/products/categories/platform/techbiz/overview.html) service management tool. Until 30th June, you can still use CFT free of charge.

|      Date      |    Details     |
|------------------------|-----------------------|
|     **16 June 2025**        |    Start of CFT Subscription via TechBiz   |
|     **01 July 2025**         |     Billing starts for all CFT Projects subscribed via TechBiz  |
|     **31 August 2025**        |   Deadline for CFT subscription via TechBiz*      |
|    **01 September 2025**        |     Removal of CFT projects not subscribed via TechBiz**     |
| **03 October 2025** | Receipt of first invoice for the past quarter (July to September)

?> **Note:** We strongly encourage existing CFT users to complete subscription by 30 July 2025 to ensure that agencies can review July usage in time for any necessary AOR or budgetary adjustments ahead of end-of-quarter invoicing.

\* This deadline is for existing CFT projects. New users or projects may continue to subscribe via TechBiz after this date.

** CFT will assume that projects not subscribed via TechBiz by 1 September 2025 are no longer required and may remove them without further notice.


## Pricing Feedback

 **Do you have feedback on the CFT indicative pricing?**

We kindly request your assistance in obtaining feedback from your stakeholders on our proposed pricing structure. Please reach out to **Brian Liu (Product Manager)** at brian_liu@tech.gov.sg.

## Ready to onboard?

If you have completed reviewing CFT pricing details, you can proceed to the next steps for onboarding to CFT.

- Prepare for TechBiz onboarding: [go.gov.sg/cft-onboarding-reqs](https://go.gov.sg/cft-onboarding-reqs)

- Subscribe to CFT via TechBiz portal: [go.gov.sg/cft-onboarding-steps](https://go.gov.sg/cft-onboarding-steps)

