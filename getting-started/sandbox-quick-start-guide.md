# Introduction

**Cloud File Transfer (CFT)** [sandbox](glossary.md) is a test environment that is almost identical to our production environment. Here, you can try out the CFT APIs for file transfers between systems in the internet zone.

In the following sections, you will learn how to use our APIs in the CFT sandbox to send and receive files. You will also find resource information on each of the API endpoints, request headers, response schema, sample request and response payloads!

Try out our APIs and start transferring files in minutes!


If you haven't yet signed up, fill up the [onboarding form.](https://form.gov.sg/#!/60a4cca76179d60012cdacac/preview) 


# Getting Started

## Sandbox Account Information<a name="sandbox-account-info"></a>
Refer to your welcome email for the following:

- [API Key](glossary.md)
- [API credentials - Client Id and Secret](glossary.md)
- [API Gateway Id](glossary.md)
- Sandbox (Internet).postman_environment.json*
- CFT API v1.postman_collection.json*

?>***Requires Postman application to be installed.**

## Setup Test Environment
We recommend Postman for its user-friendly and simple interface. It supports most HTTP methods and has several status codes for you to verify your response.

### 1. Install Postman Application
Download and install [Postman.](https:///www.postman.com)

### 2. Import CFT API v1 Collection and Sandbox Environment
- Click **Collections -> Import -> Folder -> \"CFT API v1.postman_collection.json\*"**
- Click **Environment -> Import -> Folder -> \"Sandbox (Internet).postman_environment.json\*"**

?>***Refer [Sandbox Account Information.](#sandbox-account-info)**

## REST APIs and their usage
The REST APIs are implemented using HTTP Protocol. Listed below are the steps for API usage to send and receive files in the CFT sandbox.

- Step 1: **Sender** exchanges API credentials for the authorization token, refer to [Authentication.](#authentication)
- Step 2: Use the authorization token to request for a secure upload URL, refer to [Create transaction.](#create-transaction)
- Step 3: [Upload file.](#upload-file)
- Step 4: Check if the file upload is successful, refer to [View transaction status.](#view-transaction-status)
- Step 5: Send the file for scan and transfer, refer to [Commit transaction.](#commit-transaction)
- Step 6: CFT will notify **Receiver** that the file is available for download, refer to [Notification.](#notification)
- Step 7: Obtain authorization token and request for a secure download URL, refer to [Download file.](#download-file)
- Step 8: Download file and notify CFT, refer to [Send Acknowledgement.](#send-acknowledgement) **This step is optional.**

### Step 1: [Authentication]<a name="authentication"></a>
Before you can start using APIs to send and receive files, you will need to authenticate yourself. You can do this by invoking the **(GET JWT(KeyCloak)) API** and providing your **\*API credentials(Client Id and Secret).** This API supports OAuth protocol.

?>***Refer [Sandbox Account Information.](#sandbox-account-info)**



![Display Step1](./images/Sandbox_auth_1.png)

You will receive an authorization token valid for 30 mins.

This token is required to:

- Upload files
- Scan and transfer files
- Download files

#### 1.1 Resource information
|Method      |POST
|------------|---------------------------
|URL         |https://api-sandbox.gdscft.govtechstack.sg/sandbox/v1/cft/auth
|What it does|Provides a short-lived authorization token to call CFT APIs

#### 1.2 HTTP request
##### 1.2.1 Headers* 
|Name             |Type     |Purpose                                  
|------------     |---------|------------                             
|x-api-key        |String   |API Key assigned to an individual project
|Authorization    |String   |Basic Auth of Client Id and Secret
|x-apigw-api-id   |String   |API Gateway Id**

?>**\*Unless mentioned otherwise, all headers are required.**

?>****API Gateway Id needs to be physically entered if not hardcoded by Postman.**


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

### Step 2: [Create transaction]<a name="create-transaction"></a>
Use this API to receive a secure URL to upload your files. Provide the authorization token (obtained earlier), name of the files to be uploaded (required), and their md5Checksum (optional)*.

You will receive a "transaction_id" and a secure URL valid for 30 minutes.

?>***To get md5Checksum, use the following command: openssl md5 -binary \[fileName\] | base64.**

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
    

### Step 3: [Upload file]<a name="upload-file"></a>
Upload your file to the URL obtained in the previous section. (Section 2 - Create transaction)
```
curl --location --request PUT '[uploadUrl]' \
--header 'Content-MD5: [md5Checksum]' \
--header 'Content-Type: text/plain' \
--data-binary '/path/[fileName]'
```

### Step 4: [View transaction status]<a name="view-transaction-status"></a>
To know if the file has been successfully uploaded, invoke the **\(Transaction Status API\).** Provide your authorization_token in the request body.

You will receive a status of the transaction including the uploaded files.

?>**Important: If there is more than one file, all files need to be uploaded before you call the Transaction Status API.**


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
|fileTimestamp*      |String      

\* fileTimestamp is optional.

##### 4.3.3 Sample response

```
{
    "projectId": "abcd",
    "transactionid": "3cfcb009-954d-4cbc-b941-855f04e87db6",
    "filesCount": 2,
    "fileDetails": [
        {
            "fileName": "test1.txt",
            "fileStatus": "FileDownloaded",
            "transactionTimestamp": "2021-02-02 10:45:02",
            "fileTimestamp": "2021-02-02 17:42:24"
        },
        {
            "fileName": "test2.pdf",
            "fileStatus": "FileDownloaded",
            "transactionTimestamp": "2021-02-02 10:45:02",
            "fileTimestamp": "2021-02-02 17:42:24"
        }
    ]
}
```

### Step 5: [Commit transaction]<a name="commit-transaction"></a>
After checking that the file has uploaded successfully, use the **\(Scan Transaction API\)** to commit the file for scan and transfer.

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
    "projectId": "abcd",
    "transactionid": "47e1da94-6c26-432f-9585-9f8927a71892",
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

### Step 6: [Notification]<a name="notification"></a>
After the scan and transfer is complete, files will be available for download. CFT system will send a notification to the Receiver via Webhook.*

?>***Webhook needs to be configured by the Receiver.**


### Step 7: [Download File]<a name="download-file"></a>
You need to obtain secure URLs to download the file, use the **Download Transaction API.**

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
|downloadUrl\*        |String       
|validFor\*           |String      

?>**\* Only available when file is copied to clean bucket.**

##### 7.3.3 Sample response payload

```

"projectId": "gt-enp",
    "transactionId": "2a94862a-2a5e-4e85-bf38-98a587c08da1",
    "filesCount": 1,
    "downloadUrls": [
        {
            "fileName": "abcd.txt",
            "fileStatus": "FileCopiedToCleanBucket",
            "downloadUrl": "https://gt-enp-clean-sandbox-internet.s3-sandbox.gdscft.govtechstack.sg/2a94862a-2a5e-4e85-bf38-98a587c08da1/abcd.txt?AWSAccessKeyId=ASIAUDIC7Y3DCGCP26R7&Expires=1629276790&Signature=zC7ncFeVSZ4iFHOuF5K%2FQhSpc%2Bw%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEGEaDmFwLXNvdXRoZWFzdC0xIkcwRQIhAJGXKuck2d343bnUF8MyGS0jPNywYSCzh8sK%2B8bSimq3AiAe6fyFwJ22ewxKJSmR4xbdSN1nRUL0wfrvlA8%2BUVs%2FFCq2AgiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAEaDDI4MTg2MzUwNTYwNiIM5CJHSuHnYhjVFNBVKooCvmokdCGCJJHcy5q7TYt8OPv%2FHD642SvzHSemeW7hV2%2F643sJniww4EcWAGInH9Udtxm3q9Ch7J8aEjvqL7jcFXNLePFoRaOa5nh6AeCo2tSQhPgXsKsvnAJesXVH8TmxQlv41YOo7%2BO%2BEgWTDbrCuBIgDxiCmCQHXluPy9MD2w9k3wTLlY%2FYPOXcuttVf88kO83zZ3alQD63jmlDMdbe63eW0n0bZwVUWiNwEr12N%2Bp6PoDWs1%2F%2F5k%2Bmt7O2LYCAjQxIYjggh8iIN4QRhjfA6oBZGQD99Xy%2FhmOFXyvtFLQDC%2BSXmvU9BNoFwLZClmMEqxyUOdXxvZbQT8QFdhxdzRZwWRMo%2FUysg90w7YbziAY6mgHOYRAbpM3vPjXcqDPi9%2BsQaP6YUm%2FOeanciG6hj3PahNL8cbML48MVAdr2jaVEeX4s9IffgY55jIxrz9vLqudBWrFUaVAntOh49j%2FOaPr%2Fwss8etDrekssJqp3MBp7VAJ01UKheq3dGbgNlbevMxuHpU9sIZCvaqHV0yQ06YBIZoHOpCowZaSmCL30Fo4hHYMjlxf5wJjULxxd",
            "validFor": "30 Minutes"
        }
    ]
}

```

### Step 8: [Send Acknowledgement]<a name="send-acknowledgement"></a>
Optional: Use the acknowledgment API to notify CFT about the files downloaded.

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