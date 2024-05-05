# SFTP Client Firewall Rules Testing (Intranet)

## Prerequisites

- Use the following endpoint values.

    | Zone | Authentication Method | Endpoint | 
    | -- | -- | -- |
    | Intranet | SSH Key Only | `sftp.in.cft.stack.gov.sg` |
    | Intranet | SSH Key and Password | `sftp-pw.in.cft.stack.gov.sg` | 

- Use any of the following supported SFTP Clients:

   - OpenSSH (macOS and Linux)<br>
   - WinSCP (Microsoft Windows only)<br>
   - Cyberduck (Windows, macOS, and Linux)<br>
   - FileZilla (Windows, macOS, and Linux)<br>
   - Tectia Version 6.5.1 (Microsoft Windows only)

##  Firewall rules testing 

To perform firewall rules testing, enter the following commands using **your SFTP client command prompt**:

1. First, enter these commands to resolve CFT SFTP FQDN through WOG DNS server.

    ```
    nslookup {endpoint}
    ```

    ```
    nslookup {endpoint} 10.120.1.103 (forwarder)
    ```

    ```
    nslookup {endpoint} 10.122.1.103 (forwarder)
    ```

    Refer to the [endpoint values](#prerequisites).

    **Expected result**

    Verify that you get the following expected result.

    - For SSH only:

        ```
        Non-authoritative answer:
        Name:  sftp.in.cft.stack.gov.sg
        Addresses:  10.211.0.134
                    10.211.0.157
        ```
    - For SSH + password:

        ```
        Non-authoritative answer:

        Name:    sftp-pw.in.cft.stack.gov.sg
        Addresses:  10.211.0.138
                    10.211.0.155
        ```

2. Enter the following command to test the connection to the CFT SFTP server.

    ```
    telnet <CFT SFTP server IP address> 22
    ```

     If the connection fails, you will see an error message indicating the reason.

<!-- 
# Connectivity test

Enter the following command to connect to the CFT SFTP Server:

```
sftp <username>@<hostname>
```

username isYour SFTP Username. This is generated in Step 1: Set up the application.

-->