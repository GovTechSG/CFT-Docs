# Tenant Whitelisting Requirements

Refer to this page for the whitelisting requirements needed to access CFT intranet or internet.

- To connect to CFT zones, do the following:
    - [Request CFT **internet** zone whitelisting](#connect-to-cft-internet-zone)
    - [Request CFT **intranet** zone whitelisting](#connect-to-cft-intranet-zone)
- To allow CFT webhooks and CFT SFTP Client to connect to your tenant system:
    - [Whitelist CFT endpoints on your tenant/agency firewalls](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/firewall-clearance)

## Connect to CFT Internet zone

If your system needs to connect to CFT internet zone, CFT needs to whitelist your Public IP address. Perform the action required below.

!> **Note:** This is only required for accessing CFT APIs from locations outside of Singapore.

| Agency's System | CFT Zone | Action required |
|---|---|---|
| Internet, Public SaaS, Commercial Cloud | Internet | Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) and provide your **Public IP address** for whitelisting. |

## Connect to CFT Intranet zone

If your system needs to connect to CFT intranet zone, perform the action required based on your system environment.

| Agency's System | CFT Zone | Action required |
|---|---|---|
| GPC, GDC, Agency DC (GEN) | Intranet | Raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg. |
| GCC1.0, GCC2.0 on Google Cloud/Azure Cloud | Intranet | Raise a CLZ Firewall Whitelisting request to GovTech AFM SR Admin at afm_sr_admin@tech.gov.sg. |
| GCC1.0, GCC2.0 on AWS | Intranet | Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup. If your CIDR IPs have been migrated from GCC1.0 to GCC2.0, there is no need to set up VPC Private Link. |

To test the incoming connectivity from tenant to CFT intranet, refer to:
- [HTTPS Firewall Rules Testing (Intranet)](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/https-firewall)
- [SFTP Client Firewall Rules Testing (Intranet)](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/sftp-firewall)

<!-- 

| Internet zone | Intranet zone |
|---|---|
| If you are calling APIs from the internet zone:<br><br>• Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) and provide your **Public IP** address for whitelisting. <br><br>• Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup.<br><br>• Whitelist CFT’s Public IP address if you enabled Webhook Notifications in the admin portal. | If you are calling APIs from the intranet zone: <br><br>• Please raise your CLZ Firewall Whitelisting request to **GovTech AFM SR Admin** at afm_sr_admin@tech.gov.sg for the following cases:<br>&nbsp;&nbsp; - Agency needs to connect CFT Intranet APIs from GEN network. <br>&nbsp;&nbsp; - Agency’s SFTP client needs to connect  CFT Intranet SFTP server from GEN network. <br>&nbsp;&nbsp;- CFT’s SFTP client needs to connect to Agency’s SFTP server in GEN network<br>• If your CIDR IPs have been migrated from GCC1.0 to GCC2.0, no need to setup VPC Private Link. <br><br>**Note:** This is also required if you enabled Webhook Notifications in the admin portal. |

-->

## What's next

You may need to allow or [whitelist CFT endpoints on your tenant/agency firewalls](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/firewall-clearance ).