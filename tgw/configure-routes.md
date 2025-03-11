# Configuring static routes for routing via GCCI Common Services Transit Gateway to CFT

Consumers and Publishers on GCC 2.0 AWS Intranet Compartments to configure static routes for routing via GCCI Common Services Transit Gateway to CFT.

Follow these steps:
1.	Identify your Intranet Subnet(s) in a GCC 2.0-provisioned Intranet VPC, that are hosting the resources interfacing with CFT, either as a Consumer, or as a Publisher.
a. The Intranet Subnet(s) CIDR range must be provided by GCC 2.0 team.
b. The Intranet Subnet(s) must have a CIDR range that is either in the 10.211.0.0/16 or 10.219.0.0/16, or 10.209.64.0/18. or 10.188.0.0/16 supernet.
c. The Intranet Subnet(s) must not be in the range 100.x.x.x.
 
Example of an Intranet subnet in a GCC 2.0 Intranet VPC within the 10.211.0.0/16 supernet
2.	Identify the specific Route table(s) that are associated with your Intranet Subnet(s) containing the resources that are interfacing with CFT, either as a Consumer, or as a Publisher.
a. The relevant Route table(s) must not be the Main Route table, which cannot be edited.
 
Example of a potentially correct route table with Main set to “No”
b. The relevant Route table(s) will usually (not always) include a route to tgw-08ae6ac62a2d95bcf (GCCI Intranet Transit Gateway for GEN Connectivity).
c. If you do not have a “non-Main” Route table(s) associated with the relevant Intranet Subnet(s), you would first have to create the “non-Main” Route table(s) and associate it with the relevant Intranet Subnet(s).

3.	Configure the static routes on the relevant Route table(s) to route to CFT Intranet subnet(s) via GCCI Common Services Transit Gateway (tgw-047cffe7907f10f3f).
a. For routing to CFT Staging Intranet VPC, please configure the following static route on the relevant Route table(s).
Destination: 10.209.93.128/25
Target: Transit Gateway tgw-047cffe7907f10f3f
b. For routing to CFT Production Intranet VPC, please configure the following static route on the relevant Route table(s).
Destination: 10.211.0.128/26
Target: Transit Gateway tgw-047cffe7907f10f3f
c. If you encounter the following error message, it means you have tried to configure the static routes on a Main Route table. Please refer to Step 2 to select or create a Non-Main Route table.
 

Important: Please do not configure the following static routes on the Route table(s) for your Intranet subnet(s). This might impact your system’s Intranet access to GEN network.
- Destination: 10.0.0.0/<X>
- Target: Transit Gateway tgw-047cffe7907f10f3f

