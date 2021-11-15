# Introduction

**Cloud File Transfer (CFT)** is an API-driven service in Singapore Government Tech Stack (SGTS) that powers secure, compliant cross-zone file transfers.

The CFT sandbox is a test environment that is almost identical to our production environment. Here, you can try out the CFT APIs for file transfers between systems in the internet zone.

In the following sections, you will learn how to use our APIs in the sandbox.

Try out our APIs and start transferring files in minutes!

If you haven't yet signed up, fill up the [onboarding form.](https://form.gov.sg/#!/60a4cca76179d60012cdacac/preview) 


# Getting Started

## What you will need
Before you start testing, ensure you have the following:

1. A sandbox account - Required for access to APIs in the test environment.
2. Postman tool - For testing purposes.

### Sandbox Account
On signup, a sandbox account will be created. You will receive a welcome email containing:

- [API Key](glossary.md)
- [API credentials - Client Id and Secret](glossary.md)
- [API Gateway Id](glossary.md)
- The test environment - Sandbox (Internet).postman_environment.json
- The test API collection - CFT API v1.postman_collection.json


### Postman Tool

1. Get Postman from [downloads.](https:///www.postman.com)

2. Create a Postman account.

3. Login and import the test environment and test API collection received during [signup](#sandbox-account)
- Click **Collections -> Import -> Folder -> \"CFT API v1.postman_collection.json\"**
- Click **Environment -> Import -> Folder -> \"Sandbox (Internet).postman_environment.json\"**

Your Postman GUI should now look like this:

![Display Postman_environment](./images/Postman_envt.png)

## REST APIs and their usage
The REST APIs are implemented using HTTP Protocol. Listed below is the API usage sequence to send and receive files in the sandbox.

**SEND FILES**
- Step 1: You will need to authenticate yourself to access the APIs. Exchange your API credentials (received during [signup](#sandbox-account)) for an authorization token, refer to [Authentication.](#authentication)
- Step 2: Use the authorization token to request for a secure upload URL, refer to [Create transaction.](#create-transaction)
- Step 3: [Upload file.](#upload-file)
- Step 4: Check if the file upload is successful, refer to [View transaction status.](#view-transaction-status)
- Step 5: Send the file for scan and transfer, refer to [Commit transaction.](#commit-transaction)

**RECEIVE FILES**
- Step 6: You will be notified that the file is available for download, refer to [Webhook notification.](#notification)
- Step 7: Obtain authorization token and request for a secure download URL, refer to [Download file.](#download-file)
- Step 8: Download file and notify CFT, refer to [Send Acknowledgement.](#send-acknowledgement) **This step is optional.**

## Standard HTTP Status Codes
Communication with the APIs is performed through HTTPS, and responses are conveyed through HTTP response codes. 

|Status Code| Description|
|------    | --------   |
|200 OK      | When the request is completed successfully and returns the response body|
|201 Created| When the request has been fulfilled and results in a new resource being created|
|400 Bad Request| When mandatory fields (headers / query parameters / body ) are missing or incorrect|
|401 Unauthorized|When Authorization credentials (token) are  missing or incorrect or expired|
|403 Forbidden|When access not configured for API endpoints|
|404 Not Found|When API resource or endpoints are not available|
|500 Internal Server Error| When an internal error occurs|

### Step 1: Authentication
Invoke the **Auth-Get JWT (Keycloak) API** and provide your [API credentials](#sandbox-account), as shown in the image below. The method and endpoint URL is automatically populated. This API supports OAuth protocol. 


![Display Step1](./images/Sandbox_auth_1.png)

Click **Send.** You will receive an authorization token valid for 30 mins.

This token is required to:

- Upload files
- Scan and transfer files
- Download files

In the following sections you will find resource information on each of the API endpoints, their request headers, response schema, sample requests and response payloads!

#### 1.1 Resource information
|Method      |POST
|------------|---------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/auth
|What it does|Provides a short-lived authorization token to call CFT APIs

#### 1.2 HTTP request
##### 1.2.1 Headers
?>**Note: Unless mentioned otherwise, all headers are required.**

|Name             |Type     |Purpose                                  
|------------     |---------|------------                             
|x-api-key        |String   |API Key assigned to an individual project
|Authorization    |String   |Basic Auth of Client Id and Secret
|x-apigw-api-id   |String   |API Gateway Id **(To be entered manually if not hardcoded by Postman)**

##### 1.2.2 Sample request
You can direcly invoke the APIs in the collection or use cURL commands as shown below:

```
curl --location --request POST 'https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/auth' \ 
--header 'x-api-key: [API Key]' \ 
--header 'x-apigw-api-id: [API Gateway Id]' \ 
--header 'Authorization: Basic [Client Id and Secret]' 
```

#### 1.3 HTTP response
##### 1.3.1 JSON schema
|Name                    |Type     
|------------            |---------
|authorization_token     |String
|expires_in              |Integer
|token_type              |String


##### 1.3.2 Sample response payload

```
{
  "authorization_token": “authorizationtoken”, 
  "expires_in": 1800, 
  "token_type": "Bearer"
}

```
The response and [status code](#standard-http-status-codes) is shown in a separate window within the GUI.

![Display Response](./images/Auth_resp.png)

### Step 2: Create transaction
After being authenticated successfully, you need to request for a secured URL to upload your files. Invoke the **Create Transaction API** and provide the following in the request body:
- Name of the files to be uploaded (required).
- Their md5Checksum (optional). To get md5Checksum, use the following command: **openssl md5 -binary \[fileName\] | base64.**

?>**Important - The authorization token, API Key and API Gateway Id are automatically included in the request header.**

![Display Step2](./images/Create_transaction.png)

#### 2.1 Resource information
|Method      |POST
|------------|----------------------------------------------------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions
|What it does|Creates a transaction and returns secured URLs to upload files

#### 2.2 HTTP request
##### 2.2.1 Headers
|Name                |Type     |Purpose                                  
|------------        |---------|------------                             
|x-api-key           |String   |API Key assigned to an individual project
|authorization_token |String   |Authorization token
|x-apigw-api-id      |String   |API Gateway Id

##### 2.2.2 JSON schema 
|Name             |Type             |Purpose
|------------     |---------        |---------
|fileDetails      |JSON array       |fileDetails JSON schema 
|fileName         |String           |file names to be uploaded with extension
|md5Checksum      |String           |md5checksum of the file

##### 2.2.3 Sample request payload
```
curl --location --request POST 'https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions' \
--header  'Authorization: [authorization_token]' \
--header 'Content-Type: application/json' \
--header 'x-api-key: [API Key]' \
--header 'x-apigw-api-id: [API Gateway Id]' \
--data-raw '{
  "fileDetails": [
    {
      "fileName": "[fileName]",
       "md5Checksum": "[md5Checksum]"
    }
  ]
}'
```

#### 2.3 HTTP response
##### 2.3.1 JSON schema
|Name           |Type                    
|------------   |---------              
|projectId      |String                 
|transactionId  |Integer                
|filesCount     |Integer                
|uploadUrls     |uploadUrls JSON schema

##### 2.3.2 uploadUrls JSON schema
|Name           |Type                    
|------------   |---------              
|fileName       |String                 
|uploadUrl      |String                 
|validFor       |String                 

##### 2.3.3 Sample Response Payload

```
{ 
    "projectId": "projectId",
    "transactionId": "transactionId",
    "filesCount": 1,
    "uploadUrls": [
        {
            "fileName": "fileName",
            "uploadUrl": "uploadUrl",
            "validFor": "30 Minutes"
    }
    ]
    }
```
    

### Step 3: Upload file
Upload your file to the URL obtained in **Step 2.**

Invoke the **Upload test1.txt API** and input your filename in the request body as shown in image below.

![Display Step3](./images/File_upload.png)

Or use the cURL command:

```
curl --location --request PUT '[uploadUrl]' \
--header 'Content-MD5: [md5Checksum]' \
--header 'Content-Type: text/plain' \
--data-binary '/path/[fileName]'
```

### Step 4: View transaction status
To know if the file has been successfully uploaded, invoke the **Transaction Status API.**

You will receive a status of the transaction including the uploaded file, the transaction timestamp and file timestamp.

?>**Important: If there is more than one file, all files need to be uploaded before you call the Transaction Status API.**

![Display Step4](./images/Transaction_status.png)

#### 4.1 Resource information
|Method      |GET
|------------|-----------------------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions/{transactionid}/status
|What it does|Returns the status of all files uploaded in a transaction

#### 4.2 HTTP request
##### 4.2.1 Headers
|Name                |Type     |Purpose                                  
|------------        |---------|------------                             
|x-api-key           |String   |API Key assigned to an individual project
|authorization_token |String   |Authorization token
|x-apigw-api-id      |String   |API Gateway Id

##### 4.2.2 Sample Request Payload
```
curl --location --request POST 'https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions/{{session_id}}/status' \
--header 'x-api-key: [API Key]' \
--header 'x-apigw-api-id: [API Gateway Id]' \
--header 'Authorization: [authorization_token]'

```
#### 4.3 HTTP response
##### 4.3.1 JSON schema
|Name           |Type                             
|------------   |---------                        
|projectId      |String                          
|transactionId  |String                           
|filesCount     |Integer                          
|fileDetails    |JSON array of fileDetails schema

##### 4.3.2 fileDetails JSON schema
|Name                 |Type         
|------------         |---------    
|fileName             |String       
|fileStatus           |String       
|transactionTimestamp |String       
|fileTimestamp (optional)     |String      

##### 4.3.3 Sample response

```
{
    "projectId": "sb-filesg",
    "transactionId": "24bafb97-79c9-4581-8ff6-8d3b6f9f5db5",
    "filesCount": 1,
    "fileDetails": [
        {
            "fileName": "test1.txt",
            "fileStatus": "FileReceived",
            "transactionTimestamp": "2021-11-11 21:21:57",
            "fileTimestamp": "2021-11-11 21:24:28"
        }
    ]
}
```

### Step 5: Commit transaction
After checking that the file has been successfully uploaded, use the **Scan Transaction API** to commit the file for scan and transfer.

![Display Step5](./images/Scan_transaction.png)

#### 5.1 Resource information
|Method      |POST
|------------|--------------------------------------------------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions/{{sessionid}}/scan
|What it does|Commits the transaction for scan and transfer

#### 5.2 HTTP request
##### 5.2.1 Headers
|Name                |Type     |Purpose                                  
|------------        |---------|------------                             
|x-api-key           |String   |API Key assigned to an individual project
|authorization_token |String   |Authorization token
|x-apigw-api-id      |String   |API Gateway Id


#### 5.3 HTTP response
##### 5.3.1 JSON schema
|Name           |Type                             
|------------   |---------                        
|projectId      |String                           
|transactionId  |String                           
|filesCount     |Integer                          
|fileDetails    |JSON array of fileDetails schema 

##### 5.3.2 fileDetails JSON schema
|Name                 |Type        
|------------         |---------    
|fileName             |String       
|fileStatus           |String       

##### 5.3.3 Sample response

```
{
    "projectId": "sb-filesg",
    "transactionId": "24bafb97-79c9-4581-8ff6-8d3b6f9f5db5",
    "filesCount": 1,
    "fileDetails": [
        {
            "fileName": "test1.txt",
            "fileStatus": "FileSubmittedForScan"
        }
    ]
}
```

### Step 6: Notification
Once the scan and transfer is complete, file will be available for download. 
CFT system will send a notification with download URL to the Receiver via Webhook.

#### 6.1 JSON schema

|Name   |Type   |
|-------|-----|
|projectId|String|
|transactionid|String|
|filesCount|Int|
|downloadUrls|JSON array of downloadUrls schema|

#### 6.2 DownloadUrls JSON schema

|Name|Type|
|-----|----|
|fileName|String|
|fileStatus|String|
|downloadUrl (available only when file is copied to Clean bucket)|String|

#### 6.3 Sample response payload


```

{
  "projectId": "mccy-p1",
  "transactionId": "ee5bf325-29cc-4e90-bdea-b7b6387cd789",
  "filesCount": 3,
  "downloadUrls": [
    {
      "fileName": "filename3.zip",
      "fileStatus": "FileCopiedToDirtyBucket"
    },
    {
      "fileName": "filename1.txt",
      "fileStatus": "FileCopiedToCleanBucket",
      "downloadUrl": "https://mccy-p1-dirty.s3.ap-southeast-1.amazonaws.com/ee5bf325-29cc-4e90-bdea-b7b6387cd789/filename1.txt?AWSAccessKeyId=ASIAVZ5BTEUY3UINHO4O&Expires=1575966669&Signature=2QVNPXn3OWpHs9ahRitVVw9VKxo%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDmFwLXNvdXRoZWFzdC0xIkYwRAIgUpCtnX90dwQKVZmr7gFETk2EiEuYpVOrO0cY3Nytgv8CICipPb1vpLK7Siqv3U2v5KgjMy9A7krWD0ZdxcZz8FYBKt8BCNH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQARoMMzk5MjMzOTgzNzkzIgzlE8SqM%2BAwZOhYc8QqswEw5uqg1EESwCruR8NARs%2FgyrjwjTky69wH1vzY7fETLeK3rromIOZUqlZ1iuz%2BwO%2BwPl9LyL3xgBAleRPCjW0U%2F9SvsDZG1eU3TUmQCMPL6bSyOjActlJHfo0BkRjRXTTGWRdGOWs8uMV0H7IUUOfpB4w61TtSGXye7U5ia0i%2By9%2B4Y5Vh%2Bj1a8CeSSxCGHYNn01N3UiYt9G0dodH%2F%2FFii3RIfC0hD%2FUWp7Ct6hxZCVBj2cjDRkr3vBTrhAZtpwWHXbyZzW7eCM1nZOHF3AJUrM6L%2Fb7NoXqRP3qktrp9ytjbtaRma6u6azn7MrZIV8lK7O%2FWPLDBoH%2FMdSgVHCJ6UUXoMzK%2FWJX1kpLW9U3df9%2BoxhwAfS%2BD705CneGQpCqEvqZ2WwYp2Ju0GIV8m%2F2cSQMP9mq3aU%2B%2B4U6ZpEudYcWY9OJXZdmMmNEIiEC%2BgmVDDsClgdlXXLvSiYXxFcLZKV7M2w%2FXtMwS2cKAeAotfeZae02v96Dy3cPKUiT6BkDILI2%2BBYK2WysHlEX%2B%2FLnNLi4HKcVNVQ6yfy%2BBR%2BA%3D%3D"
    },
    {
      "fileName": "filename2.pdf",
      "fileStatus": "FileCopiedToCleanBucket",
      "downloadUrl": "https://mccy-p1-dirty.s3.ap-southeast-1.amazonaws.com/ee5bf325-29cc-4e90-bdea-b7b6387cd789/filename2.pdf?AWSAccessKeyId=ASIAVZ5BTEUY3UINHO4O&Expires=1575966669&Signature=kVUlpyyTQb12G2C38PV4qULuxDI%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDmFwLXNvdXRoZWFzdC0xIkYwRAIgUpCtnX90dwQKVZmr7gFETk2EiEuYpVOrO0cY3Nytgv8CICipPb1vpLK7Siqv3U2v5KgjMy9A7krWD0ZdxcZz8FYBKt8BCNH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQARoMMzk5MjMzOTgzNzkzIgzlE8SqM%2BAwZOhYc8QqswEw5uqg1EESwCruR8NARs%2FgyrjwjTky69wH1vzY7fETLeK3rromIOZUqlZ1iuz%2BwO%2BwPl9LyL3xgBAleRPCjW0U%2F9SvsDZG1eU3TUmQCMPL6bSyOjActlJHfo0BkRjRXTTGWRdGOWs8uMV0H7IUUOfpB4w61TtSGXye7U5ia0i%2By9%2B4Y5Vh%2Bj1a8CeSSxCGHYNn01N3UiYt9G0dodH%2F%2FFii3RIfC0hD%2FUWp7Ct6hxZCVBj2cjDRkr3vBTrhAZtpwWHXbyZzW7eCM1nZOHF3AJUrM6L%2Fb7NoXqRP3qktrp9ytjbtaRma6u6azn7MrZIV8lK7O%2FWPLDBoH%2FMdSgVHCJ6UUXoMzK%2FWJX1kpLW9U3df9%2BoxhwAfS%2BD705CneGQpCqEvqZ2WwYp2Ju0GIV8m%2F2cSQMP9mq3aU%2B%2B4U6ZpEudYcWY9OJXZdmMmNEIiEC%2BgmVDDsClgdlXXLvSiYXxFcLZKV7M2w%2FXtMwS2cKAeAotfeZae02v96Dy3cPKUiT6BkDILI2%2BBYK2WysHlEX%2B%2FLnNLi4HKcVNVQ6yfy%2BBR%2BA%3D%3D"
    }
  ]
}
```

### Step 7: Download File
You need to authenticate yourself to download files from the secure URL. Invoke the **Download Transaction API.**

![Display Step5](./images/Download_transaction.png)

#### 7.1 Resource information
|Method      |GET
|------------|------------------------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions/{transactionid}/download
|What it does|Provides secured URLs to download files in a transaction

#### 7.2 HTTP request
##### 7.2.1 Headers
|Name                |Type     |Purpose                                  
|------------        |---------|------------                             
|x-api-key           |String   |API Key assigned to an individual project
|authorization_token |String   |Authorization token
|x-apigw-api-id      |String   |API Gateway Id                              

#### 7.3 HTTP response
##### 7.3.1 JSON schema
|Name            |Type                              
|------------    |---------                         
|projectId       |String                            
|transactionId   |String                            
|filesCount      |Integer                           
|downloadUrls    |JSON array of downloadUrls schema 

##### 7.3.2 downloadUrls JSON schema
|Name                 |Type          
|------------         |---------    
|fileName             |String      
|fileStatus           |String      
|downloadUrl (only available when file is copied to clean bucket)        |String       
|validFor\*           |String      


##### 7.3.3 Sample response payload

```

{
    "projectId": "sb-filesg",
    "transactionId": "24bafb97-79c9-4581-8ff6-8d3b6f9f5db5",
    "filesCount": 1,
    "downloadUrls": [
        {
            "fileName": "test1.txt",
            "fileStatus": "FileCopiedToCleanBucket",
            "downloadUrl": "https://sb-filesg-clean-sandbox-internet.s3-sandbox.gdscft.govtechstack.sg/24bafb97-79c9-4581-8ff6-8d3b6f9f5db5/test1.txt?AWSAccessKeyId=ASIAUDIC7Y3DJGUOVEOG&Expires=1636639598&Signature=GzZAEb4FFWUo%2FMrQrbPd2imDzLU%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEF4aDmFwLXNvdXRoZWFzdC0xIkgwRgIhALWd9rhlgK3eGdnzZt2Hg1sKz6Qt6NajR%2FCQh6n0cC9%2FAiEAyyo7bth4DetN7Vrss3uqJMb%2BgHI%2FY4gEqD7Hx8f5rKwqrQIIFxACGgwyODE4NjM1MDU2MDYiDJ811InNqS%2F2bpVWfiqKAiVtywyHdkTf8DpjPeKgbNGKq6CoZVh%2FukxNX1eP8GRFotX4p9lEOwkcwW%2BkQBZpSD7fGCy8S2zjXsdt66PThmGrWDn%2FimGbFGHFMuVI%2Fb7qHOVLOJPu3AXvaBcm6N1txkV%2BdvEDwZIRLSOcXZ1YNGCtB23Afwm3Fa3B677czlkQ76ISIfJn0PLTMup%2B2F1OyaQUjVsYMiAwdayMlXSWRRECr6EPGF%2Fyj4c%2B7GKDiXU83ZxleAK6CJes%2B0u9rRcw3GQeCd19OK97QmKLVSAoi8cmq6Jed14NaZEvIqUDCr9CNtC9jiS%2BJOY1xyKcETHf7tVPWhlFD0%2BNSpXPeV09SFk%2BnHgmFIJYhI3dMOS4tIwGOpkBh%2BD8WyZ1leDwPj72ooErPNdL7VRZbvAhqWDXOmKgVa2Qhcab4qnd2dMjqjfHJo2PqV2SaGEPnAuEs81gJgC1G1p7fNEBjVoGBERjwdYshyYL0O50hl0l9d8Wq84Gs60r%2FPbu6Vdwlxz%2BJA384iK3SldVa4yA0lTiDMVI7fEvA04f1M0mU%2FID6GNEQmHbqTRnSdSy9OSde5Ry",
            "validFor": "30 Minutes"
        }
    ]
}

```

### Step 8: Send Acknowledgement
**This step is optional.** Use the acknowledgment API to notify CFT about the files downloaded. 

#### 8.1 Resource information
|Method      |PUT
|------------|-------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions/{transactionid}/ack
|What it does|Notifies CFT about the files downloaded in a transaction

#### 8.2 HTTP request
##### 8.2.1 Headers
|Name                |Type     |Purpose                                  
|------------        |---------|------------                             
|x-api-key           |String   |API Key assigned to an individual project
|authorization_token |String   |Authorization token
|x-apigw-api-id      |String   |API Gateway Id  


## Support
Support is delivered over [Telegram channel](https://t.me/joinchat/Upwvp_0nIEtjOGNl) and during office hours. (Mon - Fri, 0930am - 0530pm)