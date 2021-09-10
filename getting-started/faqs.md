# Frequently Asked Questions

## Business Users

### About CFT

**What is Cloud File Transfer (CFT)?**

A centralised, fully managed file transfer service for the whole-of-government (WOG).

This service is a part of the Singapore Government Tech Stack (SGTS) and is hosted on the Government Commercial Cloud (GCC).

Core capabilities:

- Compliant cross-zone file transfers according to IM8
- Malware scanning with Content Disarm and Reconstruct (CDR)
- File Encryption for data integrity and security
- Application Programming Interface (API) access for web and mobile applications
- Event-driven with file download notifications

**What are the benefits of using CFT?**

CFT can benefit agencies who need to:

- Transfer large files across zones
- Move files securely
- Automatically scale based on their transfer needs
- Customize workflows
- Audit and monitor their file transfer activity
- Accelerate digitization of government services

**What is cross-zone file transfer?**

The IM8 policy requires applications to be separated into internet and intranet zones. Transfering files from one zone to another is refered to as a cross-zone file transfer.

**Will CFT be available for my use?**

CFT is available for government agencies, their business partners, and vendors.

**What are the different integration options?**

CFT will support the following integration options:

- CFT for Backend Systems - File transfers between systems.
- CFT for Web and Mobile Applications - File transfers between end-user and system.

**What type of encryption does CFT support?**

CFT supports industry and IM8 compliant standards for: 

- Encryption at rest:

Data is encrypted at-rest on the commercial cloud hosting environment with Amazon Web Services (AWS) Simple Storage Service Server Side Encryption Key Management Service (S3 SSE-KMS) (IM8 8.3/S2)


- Encryption in transit:

HTTPS over Transport Layer Security version 1.2 (TLS 1.2) and above (IM8 3.1/S3)

**What are the requirments to use CFT?**

[Sign up for a CFT Production](https://form.gov.sg/#!/603cff5e399059001248f7d4/preview) and we will get back to you as soon as possible.

**When will it be available?**

CFT for Backend Systems will be Generally Available in (Zen to confirm)

### Data Classification

**What data classification does CFT support?**

CFT is hosted in GCC which can support up to Restricted.

### Project Information

**How do I access CFT admin portal?**

TechPass is required to log in to the CFT admin portal.

**How do I monitor resource utilization of my projects?**

From the CFT admin portal.

**How many projects can I have?**

Each subscription is entitled to one project.

**What are the limits for the file transfers?**

An unlimited number of files and up to 1GB file size per transfer.

**How long will the files be stored in CFT?**

Fourteen days, after which the files will be purged.

**What happens if the file contains malware?**

- CFT for Backend Systems - File will be moved to dirty bucket , file status will be updated to ‘FileCopiedToDirtyBucket’ 
- CFT for Web and Mobile Applications - TBC

**I need more help. How do I contact you?**

Send us an email at [enquiries_CFT@tech.gov.sg](enquiries_CFT@tech.gov.sg) and we will respond to you as soon as possible.

### Integration

**How do I integrate CFT with my application?**

- For CFT for Backend Systems, refer to [Integration guide](integration-guide-cft-for-backend-systems) and the API specs
- For CFT for Web and Mobile Applications refer to [Integration guide](integration-guide-cft-for-web-and-mobile-applications)

### Subscription

**Where can I find the pricing information?**

Please contact [Support](mailto:enquiries_CFT@tech.gov.sg) for more information. (Zen to advise)

**How do I modify my subscription tier options?**

Please contact [Support](mailto:enquiries_CFT@tech.gov.sg) for more information.

**How do I download billing reports?**

Please contact [Support](mailto:enquiries_CFT@tech.gov.sg) for more information.

**I am interested in trying out CFT service, do you offer a trial?**

We offer a sandbox environment for you to try out our APIs and get a feel of the file transfer process.

Please follow the steps below to sign up:
1. Fill up the [onboarding form.](https://form.gov.sg/#!/60a4cca76179d60012cdacac/preview)
2. We will send you an email with the sandbox credentials.
3. Start testing with [OpenAPI specifications](https://docs.developer.gov.sg/docs/cft-rest-api-documentation/) or call the APIs from applications.

**If I am interested to subscribe, what are the next steps?**

[Sign up for a CFT production](https://form.gov.sg/#!/603cff5e399059001248f7d4/preview) to start transferring files on your live applications.

**How do I terminate my subscription?**

Please contact [Support](mailto:enquiries_CFT@tech.gov.sg) for more information.



## Customer Support

### Onboarding Issues

**I can't access CFT admin portal**

You need to be a TechPass user to log in to CFT admin portal.
If you do not have a TechPass account, click [here.](https://portal.stg.techpass.suite.gov.sg/public/home)

**My system has connectivity issues to CFT services**

Ensure that you have submitted the Firewall openings. Check your agency Firewall logs and Content Delivery Network (CDN) (if any) for any blockage.

### Login Issues

**I am unable to login to CFT admin portal?**

A TechPass account is required to access CFT admin portal. If you do not have a TechPass account, click [here.](https://portal.stg.techpass.suite.gov.sg/public/home)

If you are a TechPass user, your account may have been disabled due to inactivity. You will need to enable it.

**My TechPass account is disabled? How do I enable it?**

Your Agency Manager will need to raise a Service Request in the [TechPass portal](https://portal.stg.techpass.suite.gov.sg/public/home) to enable your account.

**How do I reset my TechPass password?**

You can reset password in the [TechPass portal](https://portal.stg.techpass.suite.gov.sg/public/home).

### Account Management Issues

**I am unable to add/remove users in my project**

You will need to have the Agency Manager role to add or remove users. Also, your users will need to have a TechPass account.

**I can't amend the user roles in my project**

You will need to have the Agency Manager role to amend user roles. Also, your users will need to have a TechPass account.

### File Transfer Issues

**I am not able to send files**

Here are a couple of troubleshooting steps:

- Check if workflow status is active on the portal.

If you need any assistance, please take note of the x-cft-trace-id request header and contact [Support](mailto:enquiries_CFT@tech.gov.sg)

**I am not able to receive files**

Here are a couple of troubleshooting steps:

- Check if workflow status is active on the portal.

If you need any assistance, please take note of the x-cft-trace-id request header and contact [Support](mailto:enquiries_CFT@tech.gov.sg)

**How will I be notified if my files fail to transfer?**

Please contact [Support](mailto:enquiries_CFT@tech.gov.sg) for more information.



