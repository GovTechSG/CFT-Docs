# SFTP Client Firewall Rules Testing

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

To perform firewall rules testing, enter the following commands via the command prompt, using [CFT endpoints](#prerequisites):

```
nslookup {endpoint}
```

```
nslookup {endpoint} 10.120.1.103 (forwarder)
```

```
nslookup {endpoint} 10.122.1.103 (forwarder)
```

### Expected result

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

<!-- 
# Connectivity test

Enter the following command to connect to the CFT SFTP Server:

```
sftp <username>@<hostname>
```

username isYour SFTP Username. This is generated in Step 1: Set up the application.

-->