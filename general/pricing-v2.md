# Cloud File Transfer (CFT) Pricing

!> CFT will implement billing through [TechBiz](https://www.developer.tech.gov.sg/products/categories/platform/techbiz/overview.html) starting **1 July 2025**. This page outlines the [CFT cost calculation](#cft-cost) using [proposed and indicative pricing rates](#understanding-proposed-and-indicative-pricing-rates) to help you plan and budget accordingly. Additional details about TechBiz onboarding will be shared soon.

?> Please be informed that while our pricing methodology and structure have been substantially finalised, the figures presented are currently undergoing review and may be subject to modification. We value your patience and understanding during this process.

<small>If you are completely new to CFT and have onboarding questions, please contact us using [this form](https://form.gov.sg/665978af9040a6be24d3978b?665982626cae0284c464a22a=Cloud+File+Transfer).
</small>

##  CFT Pricing Overview

CFT's monthly charges are calculated at the **Project level** and based on two components:

- [Component 1: File Transfer Count](#component-1-file-transfer-count)
- [Component 2: Data Volume + Feature Usage](#component-2-data-volume--feature-usage)

### Component 1: File Transfer Count

You are billed based on the total number of files transferred, as uploaded from the **Sender Application**.

- Each successful file transfer is counted, including zipped folders.
- One zipped folder is considered one file for the purposes of file transfer billing — regardless of how many files are inside.

|      Tier     |      Files in Tier          |      Price per File     |      Max Cost for Tier     |
|---------------|-----------------------------|-------------------------|----------------------------|
|     Tier 1    |     First 1,000 files       |     $1.25               |     $1,250.00              |
|     Tier 2    |     Next 9,000 files        |     $0.25               |     $2,250.00              |
|     Tier 3    |     Next 90,000 files       |     $0.05               |     $4,500.00              |
|     Tier 4    |     Beyond 100,000 files    |     $0.01               |     Based on usage         |

Example: If your project processes 120,000 files in a month:

- Tier 1: 1,000 × $1.25 = $1,250.00
- Tier 2: 9,000 × $0.25 = $2,250.00
- Tier 3: 90,000 × $0.05 = $4,500.00
- Tier 4: 20,000 × $0.01 = $200.00

➡ Total File Transfer Charges = $8,200.00

### Component 2: Data Volume + Feature Usage

Charges are also applied based on the amount of data processed and the number of features used in each workflow.

CFT Priced Feature List:
- Scanner or Scanner-Bypass (mandatory)
- Content Disarm & Reconstruction (CDR)
- Encryption
- Decryption

|      Features Used     |      Price per GB     |
|------------------------|-----------------------|
|     1 Feature          |     $1.00             |
|     2 Features         |     $1.50             |
|     3 Features         |     $2.00             |
|     4 Features         |     $2.50             |

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

1.	File Transfer Charges = $8,200.00
2.	Data Usage Charges (Rounded off):
    - 1 Feature: 29 GB × $1.00 = $29.00
    - 3 Features: 51 GB × $2.00 = $102.00
    - 4 Features: 11 GB × $2.50 = $27.50

    → Total Data Usage Charges = $158.50

➡ Total Monthly Bill = $8,358.50

### Additional notes

- Files that are classified as Sensitive-High, password-protected and/or encrypted in formats that cannot be scanned by CFT must use the Scanner-Bypass feature.
- CFT pricing is applied only for successful file transfers and processed data, tracked monthly at the Project level.

## Indicative pricing

The rates defined in the previous sections are the proposed rates that are currently pending approval.

The rates below are the indicative pricing rates that  represent the upper range of the potential approved cost. These provide a conservative estimate for AOR and budgeting purposes.

<u>File Transfer Count</u>

|      Tier     |      Files in Tier          |      Indicative Price |  
|---------------|-----------------------------|-------------------------|
|     Tier 1    |     First 1,000 files       |     $1.25 - $1.30       |
|     Tier 2    |     Next 9,000 files        |     $0.25 - $0.26       |   
|     Tier 3    |     Next 90,000 files       |     $0.05 - $0.06       |   
|     Tier 4    |     Beyond 100,000 files    |     $0.01 - $0.02       |   

<u>>Data Volume + Feature Usage</u>

|      Features Used     |    Indicative Price per GB     |
|------------------------|-----------------------|
|     1 Feature          |     $1.00 - $1.05     |
|     2 Features         |     $1.50 - $1.60     |
|     3 Features         |     $2.00 - $2.10     |
|     4 Features         |     $2.50 - $2.60     |

## Billing Timeline

From 1 July 2025, CFT will officially start billing using the  [TechBiz](https://www.developer.tech.gov.sg/products/categories/platform/techbiz/overview.html) service management tool. Until 30th June, you can still use CFT free of charge.


## Pricing Feedback

 **Do you have feedback on the CFT indicative pricing?**

We kindly request your assistance in obtaining feedback from your stakeholders on our proposed pricing structure. Please reach out to **Brian Liu (Product Manager)** at brian_liu@tech.gov.sg.