# 01-Case study

## Overview

**Contoso, Ltd**. is a manufacturing company that has offices worldwide. Contoso works with partner organizations to bring products to market. Contoso products are manufactured by using blueprint files that the company authors and maintains. 

## Existing Environment 

Currently, Contoso uses multiple types of servers for business operations, including the following: 

- File servers 
- Domain controllers 
- Microsoft SQL Server servers 

Your network contains an Active Directory forest named **contoso.com**. All servers and client computers are joined to Active Directory. You have a public-facing application named App1. App1 is comprised of the following three tiers: 

- A SQL database 
- A web front end 
- A processing middle tier 

Each tier is comprised of five virtual machines. Users access the web front end by using HTTPS only. 

## Requirements 

### Planned Changes 

Contoso plans to implement the following changes to the infrastructure: 

- Move all the tiers of App1 to Azure. 
- Move the existing product blueprint files to Azure Blob storage. 
- Create a hybrid directory to support an upcoming Microsoft 365 migration project. 

## Technical Requirements 

Contoso must meet the following technical requirements: 

- Move all the virtual machines for App1 to Azure. 
- Minimize the number of open ports between the App1 tiers. 
- Ensure that all the virtual machines for App1 are protected by backups. 
- Copy the blueprint files to Azure over the Internet. 
- Ensure that the blueprint files are stored in the archive storage tier. 
- Ensure that partner access to the blueprint files is secured and temporary. 
- Prevent user passwords or hashes of passwords from being stored in Azure. 
- Use unmanaged standard storage for the hard disks of the virtual machines. 
- Ensure that when users join devices to Azure Active Directory (Azure AD), the users use a mobile phone to verify their identity. 
- Minimize administrative effort whenever possible. 

### User Requirements 

Contoso identifies the following requirements for users: 

- Ensure that only users who are part of a group named Pilot can join devices to Azure AD. 
- Designate a new user named **Admin1** as the service admin for the Azure subscription. 
- **Admin1** must receive email alerts regarding service outages. 
- Ensure that a new user named **User3** can create network objects for the Azure subscription.