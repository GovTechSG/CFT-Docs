# Firewall Clearance Requirements

![firewall-clearances](assets/firewall-clearances.png)

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

