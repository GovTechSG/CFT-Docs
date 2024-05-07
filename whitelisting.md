# Tenant Whitelisting Requirements

This diagram illustrates CFT Firewall configuration, depicting the various connections between CFT and Tenant systems and zones.

![firewall-clearances](assets/firewall-clearances.png)

<details><summary><b>View a complete reference list of CFT endpoints</b></summary>

|  Internet  |  Intranet  |
|---|---|
| **Webhook (IP1):**<br>18.143.30.35:443 | **Webhook (IP5)**:<br>10.211.0.128/28:443<br>10.211.0.144/28:443<br>10.211.0.160/28:443<br>10.211.0.176/28:443  |
| **HTTPS API (IP2):**<br>13.215.24.12:443<br>13.251.95.103:443<br>54.179.172.253:443  | **HTTPS API (IP6)**:<br>10.211.0.128/28:443<br>10.211.0.144/28:443  |
| **SFTP Server (IP3):**<br>18.143.254.126:22<br>54.255.69.2:22<br>13.214.73.225:22  | **SFTP Server (IP7):**<br>10.211.0.128/26:22  |
| **SFTP Client (IP4):**<br>54.255.110.113  | **SFTP Client (IP8):**<br>10.211.0.128/28:22<br>10.211.0.144/28:22<br>10.211.0.160/28:22<br>10.211.0.176/28:22  |

</details>

<br>

Depending on your system and zone, perform the whitelisting steps required.

- [CFT HTTPS Server Whitelisting](#cft-https-server-whitelisting)
- [CFT SFTP Server Whitelisting](#cft-sftp-server-whitelisting)
- [CFT SFTP Client Whitelisting](#cft-sftp-client-whitelisting)
- [CFT Notification (Webhooks) Server Whitelisting](#cft-notification-webhooks-server-whitelisting)

## CFT HTTPS Server Whitelisting

| CFT Zone | Tenant Action |
|---|---|
| **Internet** | None. Whitelisting is not required because CFT APIs are public and accessible within Singapore for all public IPs.<!-- However, if you want to access CFT APIs from outside of Singapore, you need to raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) with your details.-->
| **Intranet** | If you are accessing from GPC, GDC, Agency DC (GEN network) or from GCC1.0, GCC2.0 on Google Cloud/Azure Cloud: <br><br>• Raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg and include your system details and CFT HTTPS Intranet IPs (IP6): <Br>&nbsp;&nbsp;- **10.211.0.128/28:443**<br>&nbsp;&nbsp;- **10.211.0.144/28:443**
| | If you are on GCC1.0 or GCC2.0 on AWS, raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup. |


## CFT SFTP Server Whitelisting

| CFT Zone | Tenant Action |
|---|---|
| **Internet** | Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) to whitelist **your Tenant SFTP Client** on CFT.
| **Intranet** | If you are accessing from GPC, GDC, Agency DC (GEN network) or from GCC1.0, GCC2.0 on Google Cloud/Azure Cloud: <br><br>• Raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg and include your system details and CFT SFTP Server IP (IP7): <Br>&nbsp;&nbsp;- **10.211.0.128/26:22**
| | If you are on GCC1.0 or GCC2.0 on AWS, raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup. |

## CFT SFTP Client Whitelisting

| CFT Zone | Tenant Action |
|---|---|
| **Internet** | Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) to whitelist **your Tenant SFTP Server** on CFT.
| **Intranet** | Raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg and include your system details and CFT SFTP Client IPs (IP8): <Br>&nbsp;&nbsp;- **10.211.0.128/28:22**<br>&nbsp;&nbsp;- **10.211.0.144/28:22**<br>&nbsp;&nbsp;- **10.211.0.160/28:22**<br>&nbsp;&nbsp;- **10.211.0.176/28:22**

<!--
| | If you are on GCC1.0 or GCC2.0 on AWS, raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup. | -->

## CFT Notification (Webhooks) Server Whitelisting

| CFT Zone | Tenant Action |
|---|---|
| **Internet** | None. Whitelisting is not required.
| **Intranet** | Raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg and include your system details and CFT Notification Server IPs (IP5): <Br>&nbsp;&nbsp;- **10.211.0.128/28:443** <Br>&nbsp;&nbsp;- **10.211.0.144/28:443**<Br>&nbsp;&nbsp;- **10.211.0.160/28:443**<Br>&nbsp;&nbsp;- **10.211.0.176/28:443** |


<!-- TO DELETE?
| Agency's System | CFT Zone | Action required |
|---|---|---|
| GPC, GDC, Agency DC (GEN) | Intranet | Raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg. |
| GCC1.0, GCC2.0 on Google Cloud/Azure Cloud | Intranet | Raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg. |
| GCC1.0, GCC2.0 on AWS | Intranet | Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup. If your CIDR IPs have been migrated from GCC1.0 to GCC2.0, there is no need to set up VPC Private Link. |

-->


<!-- 

| Internet zone | Intranet zone |
|---|---|
| If you are calling APIs from the internet zone:<br><br>• Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) and provide your **Public IP** address for whitelisting. <br><br>• Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup.<br><br>• Whitelist CFT’s Public IP address if you enabled Webhook Notifications in the admin portal. | If you are calling APIs from the intranet zone: <br><br>• Please raise your CLZ Firewall Whitelisting request to **GovTech AFM SR Admin** at afm_sr_admin@tech.gov.sg for the following cases:<br>&nbsp;&nbsp; - Agency needs to connect CFT Intranet APIs from GEN network. <br>&nbsp;&nbsp; - Agency’s SFTP client needs to connect  CFT Intranet SFTP server from GEN network. <br>&nbsp;&nbsp;- CFT’s SFTP client needs to connect to Agency’s SFTP server in GEN network<br>• If your CIDR IPs have been migrated from GCC1.0 to GCC2.0, no need to setup VPC Private Link. <br><br>**Note:** This is also required if you enabled Webhook Notifications in the admin portal. |

-->

## What's next

- To validate the firewall rules **from tenant system to CFT intranet**, refer to:
    - [HTTPS Firewall Rules Testing (Intranet)](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/https-firewall)
    - [SFTP Client Firewall Rules Testing (Intranet)](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/sftp-firewall)

- You may need to allow or [whitelist CFT endpoints on your Tenant/Agency Firewalls](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/firewall-clearance ).