# HTTPS Firewall Rules Testing (Intranet)

> If you have completed [whitelisting requirements](https://docs.developer.tech.gov.sg/docs/cft-additional-docs/whitelisting) and you want to test **HTTPS Client connection to CFT intranet**, follow the steps in this section.

## Prerequisites

Use the following endpoint:
- Intranet: `api.in.cft.stack.gov.sg`

## Firewall rules test using cURL

- If using the command prompt, execute the following command with the [CFT endpoint](#prerequisites):

    ```
    curl -v {endpoint}
    ```

    If the connection is successful, the result should be `200 OK`.

- If using Postman, connect to the [CFT endpoint](#prerequisites) directly.

    If the connection is successful, the result should be `200 OK`.

## Firewall rules test using nslookup

To perform DNS resolution of [CFT endpoints](#prerequisites), execute the following commands via the command prompt:

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

```
Non-authoritative answer:
Name:    api.in.cft.stack.gov.sg
Addresses:  10.211.0.134
            10.211.0.157

```
