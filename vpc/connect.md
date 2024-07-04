# Connecting to the CFT VPC Endpoint

!> **Note:**  This guide applies to tenants hosted on the **GCC1.0/2.0 AWS Intranet** compartment only.

To connect to the CFT Virtual Private Cloud (VPC) Endpoint Service, follow these steps:

1. Raise a [CFT service request](http://go.gov.sg/cft-sm) to whitelist your AWS account for access CFT VPC Endpoint. Ensure to include your **12-digit AWS Account ID** in the request. 

    Once whitelisting is completed, the CFT team will confirm via the same SR ticket.

2. After whitelisting confirmation, proceed with setting up the VPC Endpoint to utilise the CFT Intranet (API/SFTP) VPCE Service. 

    Refer to the setup details below.

    | Parameter | Value |
    |--|--|
    | Name tag | *Any desired name tag* |
    | Service Category | Other endpoint services |
    | Service name | - `com.amazonaws.vpce.ap-southeast-1.vpce-svc-085a917dea19e8abd` (API)<br>- `com.amazonaws.vpce.ap-southeast-1.vpce-svc-066531d21cca304d2`(SFTP)<br>-  `com.amazonaws.vpce.ap-southeast-1.vpce-svc-0f9a0b5d5fc6d1fc7`(SFTP using password authentication) |
    | VPC | *Select **intranet VPC** ONLY, provisioned via GCC CMP portal.* |
    | Subnets | *Any desired AZs and intranet VPC subnet IDs for the VPCE* <br><br>**Note:**  Utilising multiple AZs improves the robustness of the private link. CFT Cloud Intranet supports AZ1, AZ2, and AZ3 in the ap-southeast region. |
    | Security Groups | *Select or create an appropriate Security Group that will allow your egress resource to reach the VPCE* |
    | Tags | *Any desired tags* |

    Upon successful creation of the VPC Endpoint, CFT will receive a pending request.

3. Notify the CFT team to approve the VPCE connection request via the same SR.

4. Create a **private hosted zone on Route 53** with the follow details.

    | Parameter | Value |
    |--|--|
    | Domain name | `in.cft.stack.gov.sg` |
    | Type | Private Hosted Zone |
    | VPCs Region | Your intranet VPC region |
    | VPCs ID | Your intranet VPC ID |
    | Tags | Your desired tags |

5. Create an entry in `in.cft.stack.gov.sg` zone to map the following.

    **API**
    | Record name| Type | Routing Policy | Alias |Value/Route traffic to | TTL |
    |--|--|--|--|--|--|
    | `api.in.cft.stack.gov.sg` | CNAME | Simple | No | Your API VPCE DNS name | 300 |

    **SFTP**
    | Record name| Type | Routing Policy | Alias |Value/Route traffic to | TTL |
    |--|--|--|--|--|--|
    | `sftp.in.cft.stack.gov.sg` | CNAME | Simple | No | Your SFTP VPCE DNS name | 300 |
    | `sftp-pw.in.cft.stack.gov.sg` | CNAME | Simple | No | Your SFTP VPCE DNS name | 300 |


6. Test the connectivity. Use any of the following ways:

    - Use `nslookup` on your egress resource to verify DNS resolution:
    
        - `nslookup api.in.cft.stack.gov.sg` for API 
        - `nslookup sftp.in.cft.stack.gov.sg` for SFTP
        - `nslookup sftp-pw.in.cft.stack.gov.sg` for SFTP Password Authentication 

    - Use `curl` to attempt access to CFT Intranet API/SFTP/SFTP PW servers. A successful connection should return an HTTP response code 200.

        - `curl -vk https://api.in.cft.stack.gov.sg` for API
        - `curl -vk https://sftp.in.cft.stack.gov.sg` for SFTP
        - `curl -vk https://sftp-pw.in.cft.stack.gov.sg` for SFTP Password Authentication 

    - Ue `sftp`: `sftp <cft_sftp_server_hostname>:22`

?> **Note:** Ensure that the necessary security group rules and NACLS are allowed for connectivity between your instance and the endpoint.