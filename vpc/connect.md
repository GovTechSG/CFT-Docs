# Connecting to the CFT VPC Endpoint

!> **Note:**  This is applicable to tenants hosted on **GCC1.0/2.0 AWS Intranet** compartment only.

Follow these steps to connect to CFT Virtual Private Cloud Endpoint Service.

1. Raise a [CFT service request](http://go.gov.sg/cft-sm) to whitelist your AWS account to access CFT VPC Endpoint. Ensure to provide your **12-digit AWS Account ID** in the request. 

    CFT team will send a confirmation via the same SR ticket once whitelisting is completed. 

2. After whitelisting is completed, proceed to set up the VPC Endpoint to utilise the CFT Intranet (API/SFTP) VPCE Service.

    Refer to the setup details below.

    | Parameter | Value |
    |--|--|
    | Name tag | *Any desired name tag* |
    | Service Category | Other endpoint services |
    | Service name | - `com.amazonaws.vpce.ap-southeast-1.vpce-svc-085a917dea19e8abd` (API)<br>- `com.amazonaws.vpce.ap-southeast-1.vpce-svc-066531d21cca304d2`(SFTP)<br>-  `com.amazonaws.vpce.ap-southeast-1.vpce-svc-0f9a0b5d5fc6d1fc7`(SFTP using password authentication) |
    | VPC | *Select **intranet VPC** ONLY, provisioned via GCC CMP portal.* |
    | Subnets | *Any desired AZs and intranet VPC subnet IDs for the VPCE* <br><br>**Note:**  Multi-AZ will improve robustness of the private link. CFT Cloud Intranet supports AZ1, AZ2, and AZ3 in the ap-southeast region. |
    | Security Groups | *Select or create an appropriate Security Group that will allow your egress resource to reach the VPCE* |
    | Tags | *Any desired tags* |

    Once the VPC Endpoint is created successfully, CFT will receive a pending request.

3. Notify the CFT team to accept the VPCE connection request via the same SR.

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

    - Test via `nslookup` command on your egress resource to confirm whether the DNS resolution has taken effect.
    
        - `nslookup api.in.cft.stack.gov.sg` for API 
        - `nslookup sftp.in.cft.stack.gov.sg` for SFTP
        - `nslookup sftp-pw.in.cft.stack.gov.sg` for SFTP Password Authentication 

    - Test via `curl` command to try and access CFT Intranet API/SFTP/SFTP PW servers. A positive result would yield an HTTP response code 200.

        - `curl -vk https://api.in.cft.stack.gov.sg` for API
        - `curl -vk https://sftp.in.cft.stack.gov.sg` for SFTP
        - `curl -vk https://sftp-pw.in.cft.stack.gov.sg` for SFTP Password Authentication 

    - Test via `sftp`.
    
        `sftp <cft_sftp_server_hostname>:22`

?> **Note:** Ensure that the necessary security group rules and NACLS are allowed for connectivity between instance and endpoint.