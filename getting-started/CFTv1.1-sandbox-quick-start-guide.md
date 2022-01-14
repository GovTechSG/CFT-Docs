# Introduction

**Cloud File Transfer (CFT)** is an API-driven service in the Singapore Government Tech Stack (SGTS) that powers secure, compliant cross-zone file transfers.

The CFT sandbox is a test environment that is almost identical to our production environment. Here, you can try out the CFT APIs for file transfers between systems in the internet zone.

In the following sections, you will learn how to use our APIs in the sandbox.

# Getting Started

## What you will need
Before you start testing, ensure you have the following:

1. A sandbox account - Required for access to APIs in the test environment.
2. Postman tool - For testing purposes.

### Sandbox Account
Sign up for a sandbox account by filling up our [onboarding form.](https://form.gov.sg/#!/60a4cca76179d60012cdacac/preview)

Upon successful signup, a sandbox account will be created and you will receive a welcome email containing:

- [API Key](glossary.md)
- [API credentials - Client Id and Secret](glossary.md)
- [API Gateway Id](glossary.md)
- Postman environment file - Sandbox (Internet).postman_environment.json
- Postman API collections file - CFT API v1.postman_collection.json


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
- Step 1: You will need to authenticate yourself to access the APIs. Exchange your API credentials (received during [signup](#sandbox-account)) for an Authorization token, refer to [Authentication.](#authentication)
- Step 2: Once authenticated successfully, use Authorization token to request for a secure upload URL, refer to [Create transaction.](#create-transaction)
- Step 3: [Upload file.](#upload-file)
- Step 4: Check if the file upload is successful, refer to [View transaction status.](#view-transaction-status)
- Step 5: Send the file for scan and transfer, refer to [Commit transaction.](#commit-transaction)

**RECEIVE FILES**
- Step 6: You will be notified that the file is available for download, refer to [Webhook notification.](#notification)
- Step 7: Obtain Authorization token and request for a secure download URL, refer to [Download file.](#download-file)
- Step 8: Download file and notify CFT, refer to [Send Acknowledgement.](#send-acknowledgement) **This step is optional.**

## Standard HTTP Status Codes
Communication with the APIs is performed through HTTPS, and responses are conveyed through HTTP response codes. 

|Status Code| Description|
|------    | --------   |
|200 OK      | When the request is completed successfully and returns the response body|
|201 Created| When the request has been fulfilled and results in a new resource being created|
|400 Bad Request| When mandatory fields (headers / query parameters / body) are missing or incorrect|
|401 Unauthorized|When Authorization credentials (token) are  missing or incorrect or expired|
|403 Forbidden|When access is not configured for API endpoints|
|404 Not Found|When API resource or endpoints are not available|
|500 Internal Server Error| When an internal error occurs|

### Step 1: Authentication
Invoke the **Auth-Get JWT (Keycloak) API** and provide your [API credentials](#sandbox-account), as shown in the image below. The method and endpoint URL is automatically populated. This API supports OAuth protocol. 


![Display Step1](./images/Sandbox_auth_1.png)

Click **Send.** You will receive an Authorization token valid for 30 mins.

This token is required to:

- Upload files
- Scan and transfer files
- Download files
- Check status of the files in a transaction

In the following sections, you will find resource information on each of the API endpoints, their request headers, response schema, sample requests and response payloads!

#### 1.1 Resource information
|Method      |POST
|------------|---------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/auth
|What it does|Provides a short-lived Authorization token to call CFT APIs

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

<table>
<tr>
<td> Code </td> <td> Response </td>
</tr>
<tr>
<td> 200 </td> 
<td>

```
{
"authorization_token": "authorizationtoken",
"expires_in":1800
"token_type": "bearer"
}
```

</td>
</tr>
</table>

The response and [status code](#standard-http-status-codes) is shown in a separate window within the GUI.

![Display Response](./images/Auth_resp.png)

### Step 2: Create transaction
After being authenticated successfully, you need to request for a secured URL to upload your files. Invoke the **Create Transaction API** and provide the following in the request body:
- Name of the files to be uploaded (required)
- and md5Checksum (optional) of the file. Use CLI command: " **openssl md5 -binary \[fileName\] | base64 "** to get MD5 checksum of the file.

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
|Authorization |String   |Authorization token
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
}
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

<table>
<tr>
<td> Code </td> <td> Response </td>
<tr>
<td> 200 </td>
<td>

```
{
   "projectId":"vapt-internet",
   "transactionId":"00a2e03b-7370-4005-a7da-b3d41a78359c",
   "filesCount":2,
   "uploadUrls":[
      {
         "fileName":"test1.txt",
         "uploadUrl":"https://vapt-internet-staging-dev-internet.s3-dev.gdscft.govtechstack.sg/00a2e03b-7370-4005-a7da-b3d41a78359c/test1.txt?AWSAccessKeyId=ASIAUDIC7Y3DO3KSRKOS&Content-MD5=CY9rzUYh03PK3k6DJie09g%3D%3D&Content-Type=text%2Fplain&Expires=1640253938&Signature=YzkwolaDHnS0Aujgx08%2F6XqZx1w%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEEoaDmFwLXNvdXRoZWFzdC0xIkcwRQIhAKpsgaq5nvbHZqcE06TRut7wkmTBRdTh7KUjNDGS0I9cAiBtqZ2UP5cysqMJaze5%2ByFUqwxWLN4NGLeyU2bWSF2lHCqzAghDEAIaDDI4MTg2MzUwNTYwNiIM92Kng%2BP3KYpxIUoOKpACD7fnRe9XQwNTT0KVBW98IcEewsy9k7tWyrdJ4TREt7sh%2FtZbTHhjAP%2FmvhqD6jITOeYiSMZS%2B1sud8Mluf858FHThyqorobchjr6L%2Be%2FPwtU2W%2Bc3C6%2FWWJCqwC6xkDowU0kZh%2BPD64O4FM8SFfdWiyV9jvCVG7j8uMchzyRUlL9pwi4Yj%2F7%2FsUHn3j0gzs%2FDOimyMXCc%2BPvIz8lr1lUo1s06LcRbdEgR%2FmCrSP1y017Wo5DWvWAdHZyKEnzNWEyMtcKCEyV2r95kLEZghHrwm3r0SLmQ%2FVq1D1datiUnariwgBF0%2BNYu95RzPjd5VEete3I%2Fm65pa3di69yR3IcRG26DHAfDrHK3IUscjAzvOsw54WRjgY6mgH0BMd21NR7daESYbSfLrBqqDEnpBTRme5Q5TXjU1wYMaHn6pGK5HU3HVzD7ow%2Bc8JpRhR57XN9X0nk%2FIjgBQT5VfxNuUTxEF21CWL1IIrQtxlP%2FzDXEyAQs0E3pyIG6%2FSfhufiiH02JwVcBdV9YtlMlcjmZ%2FIr345SAKi4h4j2w1WAL7h1ifXK8tsbc%2B5fsDUtEgVBZ%2FFSj0WC",
         "validFor":"30 Minutes"
      },
      {
         "fileName":"test2.pdf",
         "uploadUrl":"https://vapt-internet-staging-dev-internet.s3-dev.gdscft.govtechstack.sg/00a2e03b-7370-4005-a7da-b3d41a78359c/test2.pdf?AWSAccessKeyId=ASIAUDIC7Y3DO3KSRKOS&Content-MD5=MGWntnyj%2BJRftE0ylgesAw%3D%3D&Content-Type=application%2Fpdf&Expires=1640253938&Signature=HykSHMEtmP%2FAGYBedY42eyl7kb8%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEEoaDmFwLXNvdXRoZWFzdC0xIkcwRQIhAKpsgaq5nvbHZqcE06TRut7wkmTBRdTh7KUjNDGS0I9cAiBtqZ2UP5cysqMJaze5%2ByFUqwxWLN4NGLeyU2bWSF2lHCqzAghDEAIaDDI4MTg2MzUwNTYwNiIM92Kng%2BP3KYpxIUoOKpACD7fnRe9XQwNTT0KVBW98IcEewsy9k7tWyrdJ4TREt7sh%2FtZbTHhjAP%2FmvhqD6jITOeYiSMZS%2B1sud8Mluf858FHThyqorobchjr6L%2Be%2FPwtU2W%2Bc3C6%2FWWJCqwC6xkDowU0kZh%2BPD64O4FM8SFfdWiyV9jvCVG7j8uMchzyRUlL9pwi4Yj%2F7%2FsUHn3j0gzs%2FDOimyMXCc%2BPvIz8lr1lUo1s06LcRbdEgR%2FmCrSP1y017Wo5DWvWAdHZyKEnzNWEyMtcKCEyV2r95kLEZghHrwm3r0SLmQ%2FVq1D1datiUnariwgBF0%2BNYu95RzPjd5VEete3I%2Fm65pa3di69yR3IcRG26DHAfDrHK3IUscjAzvOsw54WRjgY6mgH0BMd21NR7daESYbSfLrBqqDEnpBTRme5Q5TXjU1wYMaHn6pGK5HU3HVzD7ow%2Bc8JpRhR57XN9X0nk%2FIjgBQT5VfxNuUTxEF21CWL1IIrQtxlP%2FzDXEyAQs0E3pyIG6%2FSfhufiiH02JwVcBdV9YtlMlcjmZ%2FIr345SAKi4h4j2w1WAL7h1ifXK8tsbc%2B5fsDUtEgVBZ%2FFSj0WC",
         "validFor":"30 Minutes"
      }
   ]
}
```

</td>
<tr>
<td> 4xx </td> 
<td> 

{"message":"error message"} 

</td>
</tr>
</table>

### Step 3: Upload file
Upload your file using the secure URL obtained in **Step 2.**

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
|Authorization |String   |Authorization token
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

<table>
<tr>
<td> Code </td> <td> Response </td>
</tr>
<tr>
<td> 200 </td>
<td>

```
{
    "projectId": "vapt-internet",
    "transactionid": "00a2e03b-7370-4005-a7da-b3d41a78359c",
    "filesCount": 2,
    "fileDetails": [
        {
            "fileName": "test1.txt",
            "fileStatus": "FileUploaded",
            "transactionTimestamp": "2021-02-02 10:45:02",
            "fileTimestamp": "2021-02-02 17:42:24"
        },
        {
            "fileName": "test2.pdf",
            "fileStatus": "FileUploaded",
            "transactionTimestamp": "2021-02-02 10:45:02",
            "fileTimestamp": "2021-02-02 17:42:24"
        }
    ]
}
```

</td>
</tr>
<tr>
<td> 4xx </td> 
<td> 

{"message":"error message"} 

</td>
</tr>
</table> 


### Step 5: Commit transaction
After checking that the file has been successfully uploaded, use the **Scan Transaction API** to commit the file for scan and transfer.

![Display Step5](./images/Scan_transaction.png)

#### 5.1 Resource information
|Method      |POST
|------------|--------------------------------------------------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions/{{transactionid}}/scan
|What it does|Commits the transaction for scan and transfer

#### 5.2 HTTP request
##### 5.2.1 Headers
|Name                |Type     |Purpose                                  
|------------        |---------|------------                             
|x-api-key           |String   |API Key assigned to an individual project
|Authorization |String   |Authorization token
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

<table>
<tr>
<td> Code </td> <td> Response </td>
</tr>
<tr>
<td> 200 </td>
<td>

```
{
    "projectId": "vapt-internet",
    "transactionid": "00a2e03b-7370-4005-a7da-b3d41a78359c",
    "filesCount": 2,
    "fileDetails": [
        {
            "fileName": "test1.txt",
            "fileStatus": "FileSubmittedForScan"
        },
        {
            "fileName": "test2.pdf",
            "fileStatus": "FileSubmittedForScan"
        }
    ]
}
```

</td>
<tr>
<td> 4xx </td> <td> {"message":"error message"} </td>
</tr>
</table>

### Step 6: Notification
CFT system will send a notification with download URLs to the Receiver via Webhook once the files are scanned.

?>**Important: Webhook needs to be configured by Receiver.**

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
|downloadUrl |String|

?>**Important: Download URL is available only when the files are clean.**

#### 6.3 Sample response payload

<table>
<tr>
<td> Code </td> <td> Response </td>
</tr>
<tr>
<td> 200 </td>
<td>

```
{
  "projectId": "vapt-internet",
  "transactionId": "00a2e03b-7370-4005-a7da-b3d41a78359c",
  "filesCount": 2,
  "downloadUrls": [
    {
      "fileName": "filename1.txt",
      "fileStatus": "FileCopiedToCleanBucket",
      "downloadUrl": "https://vapt-internet-clean-dev-internet.s3-dev.gdscft.govtechstack.sg/ee5bf325-29cc-4e90-bdea-b7b6387cd789/filename1.txt?AWSAccessKeyId=ASIAVZ5BTEUY3UINHO4O&Expires=1575966669&Signature=2QVNPXn3OWpHs9ahRitVVw9VKxo%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDmFwLXNvdXRoZWFzdC0xIkYwRAIgUpCtnX90dwQKVZmr7gFETk2EiEuYpVOrO0cY3Nytgv8CICipPb1vpLK7Siqv3U2v5KgjMy9A7krWD0ZdxcZz8FYBKt8BCNH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQARoMMzk5MjMzOTgzNzkzIgzlE8SqM%2BAwZOhYc8QqswEw5uqg1EESwCruR8NARs%2FgyrjwjTky69wH1vzY7fETLeK3rromIOZUqlZ1iuz%2BwO%2BwPl9LyL3xgBAleRPCjW0U%2F9SvsDZG1eU3TUmQCMPL6bSyOjActlJHfo0BkRjRXTTGWRdGOWs8uMV0H7IUUOfpB4w61TtSGXye7U5ia0i%2By9%2B4Y5Vh%2Bj1a8CeSSxCGHYNn01N3UiYt9G0dodH%2F%2FFii3RIfC0hD%2FUWp7Ct6hxZCVBj2cjDRkr3vBTrhAZtpwWHXbyZzW7eCM1nZOHF3AJUrM6L%2Fb7NoXqRP3qktrp9ytjbtaRma6u6azn7MrZIV8lK7O%2FWPLDBoH%2FMdSgVHCJ6UUXoMzK%2FWJX1kpLW9U3df9%2BoxhwAfS%2BD705CneGQpCqEvqZ2WwYp2Ju0GIV8m%2F2cSQMP9mq3aU%2B%2B4U6ZpEudYcWY9OJXZdmMmNEIiEC%2BgmVDDsClgdlXXLvSiYXxFcLZKV7M2w%2FXtMwS2cKAeAotfeZae02v96Dy3cPKUiT6BkDILI2%2BBYK2WysHlEX%2B%2FLnNLi4HKcVNVQ6yfy%2BBR%2BA%3D%3D"
    },
    {
      "fileName": "filename2.pdf",
      "fileStatus": "FileCopiedToCleanBucket",
      "downloadUrl": "https:///vapt-internet-clean-dev-internet.s3-dev.gdscft.govtechstack.sg/ee5bf325-29cc-4e90-bdea-b7b6387cd789/filename2.pdf?AWSAccessKeyId=ASIAVZ5BTEUY3UINHO4O&Expires=1575966669&Signature=kVUlpyyTQb12G2C38PV4qULuxDI%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDmFwLXNvdXRoZWFzdC0xIkYwRAIgUpCtnX90dwQKVZmr7gFETk2EiEuYpVOrO0cY3Nytgv8CICipPb1vpLK7Siqv3U2v5KgjMy9A7krWD0ZdxcZz8FYBKt8BCNH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQARoMMzk5MjMzOTgzNzkzIgzlE8SqM%2BAwZOhYc8QqswEw5uqg1EESwCruR8NARs%2FgyrjwjTky69wH1vzY7fETLeK3rromIOZUqlZ1iuz%2BwO%2BwPl9LyL3xgBAleRPCjW0U%2F9SvsDZG1eU3TUmQCMPL6bSyOjActlJHfo0BkRjRXTTGWRdGOWs8uMV0H7IUUOfpB4w61TtSGXye7U5ia0i%2By9%2B4Y5Vh%2Bj1a8CeSSxCGHYNn01N3UiYt9G0dodH%2F%2FFii3RIfC0hD%2FUWp7Ct6hxZCVBj2cjDRkr3vBTrhAZtpwWHXbyZzW7eCM1nZOHF3AJUrM6L%2Fb7NoXqRP3qktrp9ytjbtaRma6u6azn7MrZIV8lK7O%2FWPLDBoH%2FMdSgVHCJ6UUXoMzK%2FWJX1kpLW9U3df9%2BoxhwAfS%2BD705CneGQpCqEvqZ2WwYp2Ju0GIV8m%2F2cSQMP9mq3aU%2B%2B4U6ZpEudYcWY9OJXZdmMmNEIiEC%2BgmVDDsClgdlXXLvSiYXxFcLZKV7M2w%2FXtMwS2cKAeAotfeZae02v96Dy3cPKUiT6BkDILI2%2BBYK2WysHlEX%2B%2FLnNLi4HKcVNVQ6yfy%2BBR%2BA%3D%3D"
    }
  ]
}
```

</td>
</tr>
<tr>
<td> 4xx </td> 
<td> 

{"message":"error message"} 

</td>
</tr>
</table>

### Step 7: Download File
Invoke the **Download Transaction API.** You will need to authenticate yourself before you can download files from the secure URL.

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
|Authorization |String   |Authorization token
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
|downloadUrl (only available when files are clean)        |String       
|validFor\*           |String      


##### 7.3.3 Sample response payload

<table>
<tr>
<td> Code </td> <td> Response </td>
</tr>
<tr>
<td> 200 </td>
<td>

```
{
    "projectId": "vapt-internet",
    "transactionid": "1c56c827-80e4-4406-b957-17b184a7a116",
    "filesCount": 2,
    "downloadUrls": [
        {
            "fileName": "test1.txt",
            "fileStatus": "FileCopiedToCleanBucket",
            "downloadUrl": "https://vapt-internet-clean-dev-internet.s3.gdscft.govtechstack.sg/1c56c827-80e4-4406-b957-17b184a7a116/test1.txt?AWSAccessKeyId=ASIAUDIC7Y3DH2EJZLR7&Expires=1612348134&Signature=9ybjpxe4Yb3%2FO%2FJ1J7CLL6SoC1s%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEAIaDmFwLXNvdXRoZWFzdC0xIkgwRgIhAJCpZJge0eLv9XOvCOkOeg%2F6met37Q5S1JV8yOb5HPn8AiEAxM3DRQQiRZoqmLtPgbjQkLj7Wytwdb3Y6hfOUD3huv0q7AEI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARABGgwyODE4NjM1MDU2MDYiDHLwguwBLcF1mJ0O9yrAAdeYlV7iaykoRG%2FaePpShC8KLPeK5PrpsGSrEmHzFYlgpuAKeowFcq8NfP66lKJzFpZ8O%2FkuFDaVvX28oT6w%2BxLLY%2F0FlEGCcfpijUccQK5YjsA2APCQKZNC84DFW2uw%2F0qsSaiDL6mFG3tsYwtgTn1lJMrJYhqEhkM2SZL1FLRYdF4bxSrBAVgszI%2BhhkZ3j96KD1Hgxp3Dh8E%2FhC%2Ba%2BpMAkyvodpaYKVF03ebTNpFd8lB0ous11ZjiCUM7ZLc4vTDd5%2BmABjrfAcIIbElfAmKZZDitza6aLETrndf6gjTZJYEgA7fR0oyjyXxvd%2FljWeIHQ7iZhH74IQoeet9KncjiH0DKIejfXVr193iEXTRF%2BFUoMZd5b%2FSmnxvfCWdEMIGK9vcMlOc4MilPUESo0v4kgBhVrspiI3d2dttrbH%2BGnb4cN6xJP28wjVMvisBYRcb5euprmhfK%2BK3Tgc8dyW8Hi3NoW1EZ7ir95HYx2Kd%2F%2F5i6h0n3g7m%2FLy%2F0t4eL1tFBtYGFWTjv0E0Gzg8fqLfvEQVpMOqY8eThRYC9KxTUD2fdIhIY1KQ%3D",
            "validFor": "30 Minutes"
        },
        {
            "fileName": "test2.pdf",
            "fileStatus": "FileCopiedToCleanBucket",
            "downloadUrl": "https://vapt-internet-clean-dev-internet.s3.gdscft.govtechstack.sg/1c56c827-80e4-4406-b957-17b184a7a116/test2.pdf?AWSAccessKeyId=ASIAUDIC7Y3DH2EJZLR7&Expires=1612348134&Signature=zx%2BaUHmzd1s8gRDL6fBbUDp3ySI%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEAIaDmFwLXNvdXRoZWFzdC0xIkgwRgIhAJCpZJge0eLv9XOvCOkOeg%2F6met37Q5S1JV8yOb5HPn8AiEAxM3DRQQiRZoqmLtPgbjQkLj7Wytwdb3Y6hfOUD3huv0q7AEI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARABGgwyODE4NjM1MDU2MDYiDHLwguwBLcF1mJ0O9yrAAdeYlV7iaykoRG%2FaePpShC8KLPeK5PrpsGSrEmHzFYlgpuAKeowFcq8NfP66lKJzFpZ8O%2FkuFDaVvX28oT6w%2BxLLY%2F0FlEGCcfpijUccQK5YjsA2APCQKZNC84DFW2uw%2F0qsSaiDL6mFG3tsYwtgTn1lJMrJYhqEhkM2SZL1FLRYdF4bxSrBAVgszI%2BhhkZ3j96KD1Hgxp3Dh8E%2FhC%2Ba%2BpMAkyvodpaYKVF03ebTNpFd8lB0ous11ZjiCUM7ZLc4vTDd5%2BmABjrfAcIIbElfAmKZZDitza6aLETrndf6gjTZJYEgA7fR0oyjyXxvd%2FljWeIHQ7iZhH74IQoeet9KncjiH0DKIejfXVr193iEXTRF%2BFUoMZd5b%2FSmnxvfCWdEMIGK9vcMlOc4MilPUESo0v4kgBhVrspiI3d2dttrbH%2BGnb4cN6xJP28wjVMvisBYRcb5euprmhfK%2BK3Tgc8dyW8Hi3NoW1EZ7ir95HYx2Kd%2F%2F5i6h0n3g7m%2FLy%2F0t4eL1tFBtYGFWTjv0E0Gzg8fqLfvEQVpMOqY8eThRYC9KxTUD2fdIhIY1KQ%3D",
            "validFor": "30 Minutes"
        }
    ]
}
```

</td>
</tr>
<tr>
<td> 4xx </td> 
<td> 

{"message":"error message"}

</td>
</tr>
</table>

### Step 8: Send Acknowledgement
Receiver app may call the acknowledgement API to notify CFT, once all the files in a transaction are downloaded successfully. **This step is optional.** 

#### 8.1 Resource information
|Method      |PUT
|------------|-------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/transactions/{transactionid}/ack
|What it does|Notify CFT once all the files are downloaded successfully in a transaction and status will be updated to "FileDownloaded".

#### 8.2 HTTP request
##### 8.2.1 Headers
|Name                |Type     |Purpose                                  
|------------        |---------|------------                             
|x-api-key           |String   |API Key assigned to an individual project
|Authorization |String   |Authorization token
|x-apigw-api-id      |String   |API Gateway Id  


## Support
Support is delivered over [Telegram channel](https://t.me/joinchat/Upwvp_0nIEtjOGNl) during office hours(Mon - Fri, 0930am - 0530pm)