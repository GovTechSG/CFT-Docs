# Configuring static routes for routing via GCCI Common Services Transit Gateway to CFT

!> **Important:** This guide applies to tenants hosted on **GCC 2.0 AWS Intranet compartment only**.

?> **Note:** If you need more help with any of the steps below, you can [reach out to us](http://go.gov.sg/cft-sm) for assistance.


To configure static routes for routing via GCCI Common Services Transit Gateway to CFT, follow these steps:

**Step 1.	Identify your Intranet Subnet(s) in a GCC 2.0-provisioned Intranet VPC that are hosting the resources interfacing with CFT, either as a Sender or Receiver**

- The Intranet Subnet(s) CIDR range **must** be provided by GCC 2.0 team.
- The Intranet Subnet(s) **must** have a CIDR range that is either in the `10.211.0.0/16` or `10.219.0.0/16`, or `10.209.64.0/18`, or `10.188.0.0/16` supernet.
- The Intranet Subnet(s) **must not** be in the range `100.x.x.x`.

    ![tgw](/assets/tgw.png)
 
    <small>Example of an Intranet subnet in a GCC 2.0 Intranet VPC within the 10.211.0.0/16 supernet</small>

**Step 2. Identify the specific Route table(s) that are associated with your Intranet Subnet(s) containing the resources that are interfacing with CFT, either as a Sender or a Receiver**

- The relevant Route table(s) **must not** be the Main Route table, which cannot be edited.
 
    ![route-table](/assets/route-table.png)

    <small>Example of a potentially correct route table with Main set to *No*</small>

- The relevant Route table(s) will **usually (not always)** include a route to `tgw-08ae6ac62a2d95bcf` (GCCI Intranet Transit Gateway for GEN Connectivity).

- If you do not have a “non-Main” Route table(s) associated with the relevant Intranet Subnet(s), you would first have to create the “non-Main” Route table(s) and associate it with the relevant Intranet Subnet(s).

**Step 3. Configure the static routes on the relevant Route table(s) to route to CFT Intranet subnet(s) via GCCI Common Services Transit Gateway (tgw-047cffe7907f10f3f)**

1. For routing to CFT Staging Intranet VPC, configure the following static route on the relevant Route table(s).
    - **Destination:** `10.209.93.128/25`
    - **Target:** `Transit Gateway tgw-047cffe7907f10f3f`

2. For routing to CFT Production Intranet VPC, configure the following static route on the relevant Route table(s).

    **Destination:** `10.211.0.128/26`
    **Target:** `Transit Gateway tgw-047cffe7907f10f3f`

If you encounter the following error message, it means you have tried to configure the static routes on a Main Route table. Please refer to Step 2 to select or create a Non-Main Route table.
 
![route-error](/assets/route-error.png)

!> **Important:** Do not configure the following static routes on the Route table(s) for your Intranet subnet(s). This might impact your system’s Intranet access to GEN network.

- **Destination:** `10.0.0.0/<X>`
- **Target:** `Transit Gateway tgw-047cffe7907f10f3f`

