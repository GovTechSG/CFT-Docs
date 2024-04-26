# Whitelisting Requirements

| S/N  | Hosting environment | Zone | Types of access |
|---|---|---|---|
| 1 | GPC, GDC, Agency DC | Intranet | Cloud Landing Zone (CLZ) |
| 2 | GCC1.0, GCC2.0 on Google Cloud/Azure Cloud | Intranet | Cloud Landing Zone (CLZ) |
| 3 | GCC1.0, GCC2.0 on AWS | Intranet | VPC Private Link Setup |
| 4 | Internet, Public SaaS, Commercial Cloud | Internet | IP whitelisting on CFT side |

| Internet zone | Intranet zone |
|---|---|
| If you are calling APIs from the internet zone:<br><br>• Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) and provide your **Public IP** address for whitelisting. <br><br>• Raise an SR via [CFT-SM](https://go.gov.sg/cft-sm) for VPC Private Link setup.<br><br>• Whitelist CFT’s Public IP address if you enabled Webhook Notifications in the admin portal. | If you are calling APIs from the intranet zone: <br><br>• Please raise your CLZ Firewall Whitelisting request to **GovTech AFM SR Admin** at afm_sr_admin@tech.gov.sg for the following cases:<br>&nbsp;&nbsp; - Agency needs to connect CFT Intranet APIs from GEN network. <br>&nbsp;&nbsp; - Agency’s SFTP client needs to connect  CFT Intranet SFTP server from GEN network. <br>&nbsp;&nbsp;- CFT’s SFTP client needs to connect to Agency’s SFTP server in GEN network<br>• If your CIDR IPs have been migrated from GCC1.0 to GCC2.0, no need to setup VPC Private Link. <br><br>**Note:** This is also required if you enabled Webhook Notifications in the admin portal. |

