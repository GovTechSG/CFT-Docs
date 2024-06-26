# Tenant Firewall Clearance

<!-- 
This diagram shows the CFT infrastructure.

![firewall-clearances](assets/firewall-clearances.png)
-->

Tenants may need to allow or **whitelist CFT IPs on your tenant/agency firewalls**.

![firewall-clearances](assets/firewall-clearances.png)

You can refer to this list of CFT IPs for firewall clearance on tenant/agency side.

|  Internet  |  Intranet  |
|---|---|
| **Webhook (IP1):**<br>18.143.30.35:443 | **Webhook (IP5)**:<br>10.211.0.128/28:443<br>10.211.0.144/28:443<br>10.211.0.160/28:443<br>10.211.0.176/28:443  |
| **HTTPS API (IP2):**<br>13.215.24.12:443<br>13.251.95.103:443<br>54.179.172.253:443  | **HTTPS API (IP6)**:<br>10.211.0.128/28:443<br>10.211.0.144/28:443  |
| **SFTP Server (IP3):**<br><u>SSH only</u><br>18.143.254.126:22<br>54.255.69.2:22<br>13.214.73.225:22<br><br><u>SSH + Password</u><br>13.228.88.235:22<br>18.142.149.152:22<br>52.221.109.108:22  | **SFTP Server (IP7):**<br>10.211.0.128/26:22  |
| **SFTP Client (IP4):**<br>54.255.110.113:22  | **SFTP Client (IP8):**<br>10.211.0.128/28:22<br>10.211.0.144/28:22<br>10.211.0.160/28:22<br>10.211.0.176/28:22  |


<!-- 

## Whitelist the CFT Notification Server IPs

To **allow webhook file transfer notifications from CFT**, whitelist the following endpoints.
- Internet: 
    - 18.143.30.35:443 
- Intranet: <br> 
    - 10.211.0.128/28:443<br>
    - 10.211.0.144/28:443<br>
    - 10.211.0.160/28:443<br>
    - 10.211.0.176/28:443

## Whitelist the CFT SFTP Client Endpoints

To **allow the CFT SFTP Client to connect to the tenant SFTP Server**, whitelist the following endpoints.
- Internet: 
    - 54.255.110.113
- Intranet: <br>
    - 10.211.0.128/28:22<br>
    - 10.211.0.144/28:22<br>
    - 10.211.0.160/28:22<br>
    - 10.211.0.176/28:22 


## Internet zone 

If your system is located in the internet zone, refer to the following CFT internet endpoints.

|   | |
| -- | -- |
| **CFT Notification Server Endpoint**<br>18.143.30.35:443 | Whitelist the CFT Notification Server Endpoint to receive webhook notifications to your system from CFT. 
| **CFT API Server**<br>13.215.24.12:443<br>13.251.95.103:443<br>54.179.172.253:443 | Test the connectivity to CFT API Server using  these endpoints.
| **CFT SFTP Server**<br>18.143.254.126:22<br>54.255.69.2:22<br>13.214.73.225:22 | Test the connectivity to CFT API Server using  these endpoints. |
| **CFT SFTP Client**<br>54.255.110.113 | |



| HTTPS | |
|---|---|
| **INTERNET** | **INTRANET** |
| **Webhook (IP1):**<br>18.143.30.35:443 | **Webhook (IP5):**<br>10.211.0.128/28:443<br>10.211.0.144/28:443<br>10.211.0.160/28:443<br>10.211.0.176/28:443 |
| **Internet endpoints (IP2):**<br>13.215.24.12:443<br>13.251.95.103:443<br>54.179.172.253:443 | **Intranet endpoints (IP6):**<br>10.211.0.128/28:443<br>10.211.0.144/28:443 |


| SFTP | |
|---|---|
| **INTERNET** | **INTRANET** |
| **CFT SFTP Server<br>endpoints (IP3):**<br>18.143.254.126:22<br>54.255.69.2:22<br>13.214.73.225:22 | **CFT SFTP Server<br>endpoints (IP7):**<br>10.211.0.128/26:22 |
| **CFT SFTP Client<br>endpoints (IP4):**<br>54.255.110.113 | **CFT SFTP Client<br>endpoints (IP8):**<br>10.211.0.128/28:22<br>10.211.0.144/28:22<br>10.211.0.160/28:22<br>10.211.0.176/28:22 |

-->