
!> CFT has upgraded from VPCe to AWS Transit Gateway (TGW). For tenants hosted on the **GCC 2.0 AWS Intranet** compartment, check the guide for [Configuring static routes for routing via GCCI Common Services Transit Gateway to CFT](/tgw/configure-routes.md).

<!-- 

# Connecting to the CFT VPC Endpoint


!> **Note:**  This guide applies to tenants hosted on the **GCC 1.0 Intranet** and **GCC 2.0 AWS Intranet** compartment only.

?> Note: If you need more help with any of the steps below, you can [reach out to us](http://go.gov.sg/cft-sm) for assistance.

To connect to the CFT Virtual Private Cloud (VPC) Endpoint Service, follow these steps:

## Step 1: Raise a whitelisting request

Raise a [CFT service request](http://go.gov.sg/cft-sm) to whitelist your AWS account for access CFT VPC Endpoint. Ensure to include your **12-digit AWS Account ID** in the request. 

Once whitelisting is completed, the CFT team will confirm via the same SR ticket.

## Step 2: Set up the VPCE

After whitelisting confirmation, proceed with setting up the VPC Endpoint to utilise the CFT Intranet (API/SFTP) VPCE Service. 

Refer to the setup details below.

| Parameter | Value |
|--|--|
| Name tag | Any desired name tag |
| Service Category | Other endpoint services |
| Service name | • API: `com.amazonaws.vpce.ap-southeast-1.vpce-svc-085a917dea19e8abd`<br><br>• SFTP: `com.amazonaws.vpce.ap-southeast-1.vpce-svc-066531d21cca304d2`<br><br>• SFTP (Password auth): `com.amazonaws.vpce.ap-southeast-1.vpce-svc-0f9a0b5d5fc6d1fc7` |
| VPC | **Intranet VPC** only, provisioned via GCC CMP portal. |
| Subnets | Any desired AZs and intranet VPC subnet IDs for the VPCE. <br><br>**Note:**  Utilising multiple AZs improves the robustness of the private link. CFT Cloud Intranet supports AZ1, AZ2, and AZ3 in the ap-southeast region. |
| Security Groups | An appropriate Security Group that will allow your egress resource to reach the VPCE |
| Tags | Any desired tags |

Upon successful creation of the VPC Endpoint, CFT will receive a pending request.

## Step 3: Notify CFT to approve the request

Notify the CFT team to approve the VPCE connection request via the same SR.


## Step 4: Create a Private Hosted Zone

Create a **private hosted zone on Route 53** with the follow details.

| Parameter | Value |
|--|--|
| Domain name | `in.cft.stack.gov.sg` |
| Type | Private Hosted Zone |
| VPCs Region | Your intranet VPC region |
| VPCs ID | Your intranet VPC ID |
| Tags | Your desired tags |

## Step 5: Add your VPCE record

Create an entry in `in.cft.stack.gov.sg` zone to map the following.

### API

| Record name| Type | Routing Policy | Alias |Value/Route traffic to | TTL |
|--|--|--|--|--|--|
| `api.in.cft.stack.gov.sg` | CNAME | Simple | No | Your API VPCE DNS name | 300 |

### SFTP

| Record name| Type | Routing Policy | Alias |Value/Route traffic to | TTL |
|--|--|--|--|--|--|
| `sftp.in.cft.stack.gov.sg` | CNAME | Simple | No | Your SFTP VPCE DNS name | 300 |
| `sftp-pw.in.cft.stack.gov.sg` | CNAME | Simple | No | Your SFTP VPCE DNS name | 300 |

## Step 6: Test connectivity

> **Notes:** <ul><li>Before proceeding with any of the options below, ensure you have completed [Step 4](#step-4-create-a-private-hosted-zone) and [Step 5](#step-5-add-your-vpce-record). Skipping these steps and calling the CFT APIs directly will result in an error.</li><li>Ensure that the necessary security group rules and NACLS are allowed for connectivity between your instance and the endpoint.</li></ul>

Test the connectivity to the VPCE through any of the following options.

### Option 1: Use nslookup commands

Use `nslookup` on your egress resource to verify DNS resolution:

- `nslookup api.in.cft.stack.gov.sg` for API 
- `nslookup sftp.in.cft.stack.gov.sg` for SFTP
- `nslookup sftp-pw.in.cft.stack.gov.sg` for SFTP (Password auth)

### Option 2: Use curl command

Use `curl` to attempt access to CFT Intranet API server. A successful connection should return an HTTP response code 200.

- `curl -vk https://api.in.cft.stack.gov.sg` for API

### Option 3: Use sftp commands

Use `sftp` to test the connection: 
- `sftp sftp.in.cft.stack.gov.sg` for SFTP
- `sftp sftp-pw.in.cft.stack.gov.sg` for SFTP (Password auth)
-->