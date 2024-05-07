# Tenant Whitelisting Requirements

Refer to this page for the whitelisting requirements needed to access CFT intranet or internet.

<!-- TO DELETE
- To connect to CFT zones, do the following:
    - [Request CFT **internet** zone whitelisting](#connect-to-cft-internet-zone)
    - [Request CFT **intranet** zone whitelisting](#connect-to-cft-intranet-zone)
- To allow CFT webhooks and CFT SFTP Client to connect to your tenant system:
    - [Whitelist CFT endpoints on your tenant/agency firewalls](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/firewall-clearance)
-->

## Connect to CFT HTTPS APIs

| CFT Zone | Tenant Action |
|---|---|
| **Internet** | Whitelisting is not required because CFT APIs are public and accessible within Singapore for all public IPs.<br><br>However, if you want to access CFT APIs from outside of Singapore, you need to raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) with your details.
| **Intranet** | If you are accessing from GEN network (GDC or GPC), raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg. <br><br>If you are on GCC1.0 or GCC2.0 on AWS, raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup. If your CIDR IPs have been migrated from GCC1.0 to GCC2.0, there is no need to set up VPC Private Link.|

<!-- TO DELETE
| Agency's System | CFT Zone | Action required |
|---|---|---|
| Internet, Public SaaS, Commercial Cloud | Internet | Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) and provide your **Public IP address** for whitelisting. | -->

## Connect to CFT SFTP Server

| CFT Zone | Tenant Action |
|---|---|
| **Internet** | Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) to whitelist your Tenant SFTP Client on CFT.
| **Intranet** | If you are accessing from GEN network (GDC or GPC), raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg. <br><br>If you are on GCC1.0 or GCC2.0 on AWS, raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup. If your CIDR IPs have been migrated from GCC1.0 to GCC2.0, there is no need to set up VPC Private Link.|

## Receive Webhook Notifications

| CFT Zone | Tenant Action |
|---|---|
| **Internet** | Whitelisting is not required.
| **Intranet** | If you are accessing from GEN network (GDC or GPC), raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg. <br><br>If you are on GCC1.0 or GCC2.0 on AWS, raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup. If your CIDR IPs have been migrated from GCC1.0 to GCC2.0, there is no need to set up VPC Private Link.|

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

- To validate the firewall rules from tenant system **to CFT intranet**, refer to:
    - [HTTPS Firewall Rules Testing (Intranet)](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/https-firewall)
    - [SFTP Client Firewall Rules Testing (Intranet)](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/sftp-firewall)

- You may need to allow or [whitelist CFT endpoints on your Tenant/Agency Firewalls](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/firewall-clearance ).