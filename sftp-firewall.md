# SFTP Client Firewall Rules Testing (Intranet)

> If you have completed [whitelisting requirements](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/whitelisting) and you want to test **SFTP Client connection to CFT intranet**, follow the steps in this section.

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


3. Alternatively, you may also use `sftp` to test the connection to the CFT SFTP server.

    ```
    sftp <cft_sftp_server_hostname>:22
    ```

    If the connection is successful, you will see response from our server, e.g., *permission denied* or a password prompt.

    If the connection is not successful, you will get a timeout.