# Configuring static routes for routing via GCCI Common Services Transit Gateway to CFT

!> **Important:** This guide applies only to tenants hosted on **GCC 2.0 AWS Intranet compartment only**.

?> **Note:** If you need assistance with any of the steps below, you can [reach out to us](http://go.gov.sg/cft-sm).

Follow these steps to configure static routes for routing via GCCI Common Services Transit Gateway to CFT:

## Prerequisite

Before you start setting up, you need to associate GEN VPC to GCCI Route 53 Profile. Refer to [GCC Documentation](
https://docs.developer.tech.gov.sg/docs/gcc-technical-documentation/hosting-model/aws-annex?id=annex-3).


## Step 1: Identify your Intranet subnet(s)

Identify the Intranet Subnet(s) in your GCC 2.0-provisioned Intranet VPC that host resources interfacing with CFT (as either Sender or Receiver). 

- The Intranet Subnet(s) CIDR range **must** be provided by the GCC 2.0 team.
- The Intranet Subnet(s) **must** have a CIDR range that is within one of the following supernets:  `10.211.0.0/16` or `10.219.0.0/16`, or `10.209.64.0/18`, or `10.188.0.0/16`.<br><br>**Important:** The application instance connecting to CFT must originate one of the CIDR ranges stated above.
- The Intranet Subnet(s) **must not** be in the range `100.x.x.x`.

    ![tgw](/assets/tgw.png)
 
    <small>Example of an Intranet subnet in a GCC 2.0 Intranet VPC within the 10.211.0.0/16 supernet</small>

## Step 2: Identify associated route tables

Identify the specific Route table(s) that are associated with your Intranet subnet(s) containing the resources that are interfacing with CFT (as either as a Sender or a Receiver).

- The relevant Route table(s) **must not** be the Main Route table, which cannot be edited.
 
    ![route-table](/assets/route-table.png)

    <small>Example of a potentially correct route table with Main set to *No*</small>

- The relevant Route table(s) will **usually (not always)** include a route to `tgw-08ae6ac62a2d95bcf` (GCCI Intranet Transit Gateway for GEN Connectivity).

- If you don't have a “non-Main” Route table(s) associated with the relevant Intranet Subnet(s), you would first have to create the “non-Main” Route table(s) and associate it with the relevant Intranet Subnet(s).

## Step 3: Configure static routes

Configure the static routes on the relevant Route table(s) to route to CFT Intranet subnet(s) via **GCCI Common Services Transit Gateway (tgw-047cffe7907f10f3f)**.

For routing to CFT Production Intranet VPC, configure the following static route on the relevant Route table(s).

**Destination:** `10.211.0.128/26`
**Target:** `Transit Gateway tgw-047cffe7907f10f3f`

If you encounter the following error message, it means you have tried to configure the static routes on a Main Route table. Please refer to Step 2 to select or create a Non-Main Route table.
 
![route-error](/assets/route-error.png)

!> **Important:** Do not configure the following static routes on the Route table(s) for your Intranet subnet(s). This might impact your system’s Intranet access to GEN network.

- **Destination:** `10.0.0.0/<X>`
- **Target:** `Transit Gateway tgw-047cffe7907f10f3f`

## Step 4: Test connectivity

Test the connectivity through any of the following options.

### Option 1: Use nslookup commands

Use `nslookup` on your egress resource to verify DNS resolution:

- `nslookup api.in.cft.stack.gov.sg` for API 
- `nslookup sftp.in.cft.stack.gov.sg` for SFTP
- `nslookup sftp-pw.in.cft.stack.gov.sg` for SFTP (Password auth)

### Option 2: Use curl command

Use `curl` to attempt access to CFT Intranet API server. A successful connection should return an HTTP response code 200.

- `curl -v https://api.in.cft.stack.gov.sg` for API

    ![https](/assets/tgw-https-success.png)

    <small>Example of a successful connectivity test</small>

### Option 3: Use sftp commands

Use `sftp` to test the connection: 
- `sftp sftp.in.cft.stack.gov.sg` for SFTP
- `sftp sftp-pw.in.cft.stack.gov.sg` for SFTP (Password auth)

    ![https](/assets/tgw-sftp-success.png)

    <small>Example of a successful connectivity test</small>


