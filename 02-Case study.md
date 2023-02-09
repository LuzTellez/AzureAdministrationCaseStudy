# 02-Case study

## Overview

**Contoso, Ltd.** is a consulting company that has a main office in Montreal and branch offices in Seattle and New York.

## Existing Environment 

Contoso has an Azure subscription named **Sub1** that is linked to an Azure Active Directory (Azure AD) tenant. The network contains an on-premises Active Directory domain that syncs to the Azure AD tenant. The Azure AD tenant contains the users shown in the following table.

| Name  | Type   | Role |
| ----- | ------ | ---- |
| User1 | Member | None |
| User2 | Guest  | None |
| User3 | Member | None |
| User4 | Member | None |

**Sub1** contains two resource groups named **RG1** and **RG2** and the virtual networks shown in the following table.

| Name  | Subnet           | Peered with  |
| ----- | ---------------- | ------------ |
| VNET1 | Subnet1, Subnet2 | VNET2        |
| VNET2 | Subnet1          | VNET1, VNET3 |
| VNET3 | Subnet1          | VNET2        |
| VNET4 | Subnet1          | none         |

**User1** manages the resources **RG1**. **User4** manages the resources in **RG2**.
**Sub1** contains virtual machines that run Windows Server 2019 as shown in the following table:

| Name | IP address  | Location   | Connected to  |
| ---- | ----------- | ---------- | ------------- |
| VM1  | 10.0.1.4    | West US    | VNET1/Subnet1 |
| VM2  | 10.0.2.4    | West US    | VNET1/Subnet2 |
| VM3  | 172.16.1.4  | Central US | VNET2/Subnet1 |
| VM4  | 192.168.1.4 | West US    | VNET3/Subnet1 |
| VM5  | 10.0.2.4    | East US    | VNET4/Subnet1 |

No network security group (NSGs) are associated to the network interfaces or the subnets.

**Sub1** contains the storage accounts shown in the following table.

| Name     | Kind                         | Location   | Files share    | Identity-based access for file share                 |
| -------- | ---------------------------- | ---------- | -------------- | ---------------------------------------------------- |
| storage1 | Storage (general purpose v1) | West US    | sharea         | Azure Active Directory Domain Services (Azure AD DS) |
| storage2 | Storage (general purpose v2) | East US    | shareb, sharec | Disabled                                             |
| storage3 | BlobStorage                  | East US    | Not applicable | Not aplicable                                        |
| storage4 | FileStorage                  | Central US | shared         | Azure Active Directory Domain Services (Azure AD DS) |

## Requirements 

### Planned Changes 

Contoso plans to implement the following changes: 

- Create a blob container named **container1** and a file share named **share1** that will use the Cool storage tier. 

- Create a storage account named **storage5** and configure storage replication for the Blob service. 

- Create an NSG named **NSG1** that will have the custom inbound security rules shown in the following table.

  | Priorty | Port | Protocol | Source      | Destination     | Action |
  | ------- | ---- | -------- | ----------- | --------------- | ------ |
  | 500     | 3389 | TCP      | 10.0.2.0/24 | Any             | Deny   |
  | 1000    | Any  | ICMP     | Any         | Virtual Network | Allow  |

- Associate NSG1 to the network interface of VM1. 

- Create an NSG named **NSG2** that will have the custom outbound security rules shown in the following table.

  | Priorty | Port | Protocol | Source      | Destination     | Action |
  | ------- | ---- | -------- | ----------- | --------------- | ------ |
  | 200     | 3389 | TCP      | 10.0.0.0/16 | Virtual Network | Deny   |
  | 400     | Any  | ICMP     | 10.0.2.0/24 | 10.0.1.0/24     | Allow  |

## Technical Requirements 

Contoso must meet the following technical requirements: 

- Create **container1** and **share1**. 
- Use the principle of least privilege. 
- Create an Azure AD security group named **Group4**. 
- Back up the Azure file shares and virtual machines by using Azure Backup. 
- Trigger an alert if **VM1** or **VM2** has less than 20 GB of free space on volume C. 
- Enable **User1** to create Azure policy definitions and **User2** to assign Azure policies to **RG1**. 
- Create an internal Basic Azure Load Balancer named **LB1** and connect the load balancer to **VNET1**/**Subnet1**.
- Enable flow logging for IP traffic from **VM5** and retain the flow logs for a period of eight months. 
- Whenever possible, grant **Group4** Azure role-based access control (Azure RBAC) read-only permissions to the Azure file shares.
