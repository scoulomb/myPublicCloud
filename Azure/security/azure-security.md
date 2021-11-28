# Azure security 

## Azure identity service 

(ps 2 todo)

### Azure active drectory (mod 3 done)


**Azure Active directory** is a SSO and application integration. It can managae 
- Azure subscription 
- Office 365
- Dynamic 365
- 3rd party app
- Own app
- Other cloud services

Azure AD is a flat strcuture

Orgsnisation usually used **Active Direcory domain services** on premise (full directory service, hierarchy structures, full features of AD such as NTLM and Kerberos)

In Azure we have **Azure Active Directory domain service**, it is a PaaS, aims (not yet the case) at replacing "active direcory domain services on premise"

We use Azure AD connect to replicate object from Active Direcory domain services to Azure active drectory
See https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect
So that use created on premise are replicated to Azure (we have to use Azure AD in Azure)



MFA : to prove who we are (something you know (password), have (device) or are (biometrics))

## RBAC 

(ps 3 ok)

- Roles allows to group together set of permission
From https://docs.microsoft.com/en-us/azure/role-based-access-control/role-definitions
> A role definition is a collection of permissions. It's sometimes just called a role. A role definition lists the actions that can be performed, such as read, write, and delete. It can also list the actions that are excluded from allowed actions or actions related to underlying data.

- User or group can be member of roles
- Members of roles inherits all permssion assigned to a role
- whne ussing roles
    - choose a built-in roles or create a role (can start from built-in)
    - assign role members
    - configure a scope for the roles

- We have 3 built-in roles
    - owner
    - contributor
    - reader


![roles](media/capture-role.png)

We can see roles and associated actions (permisisons):https://docs.microsoft.com/fr-fr/azure/role-based-access-control/built-in-roles


See here how to grant to perform role assignent:
    - user + scope
It does not seem posible to perform action assignment directly

Note on locks:
https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/lock-resources?tabs=json

> As an administrator, you can lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources. The lock overrides any permissions the user might have.

With that comes the question 
> Who can create or delete locks

> To create or delete management locks, you must have access to Microsoft.Authorization/* or Microsoft.Authorization/locks/* actions. Of the built-in roles, only Owner and User Access Administrator are granted those actions

See https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles#azure-roles

Also notes that from 
https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles#azure-roles

> The Service Administrator and Co-Administrators are assigned the Owner role at the subscription scope


It is interesting to compare a wildcard role
https://docs.microsoft.com/fr-fr/azure/role-based-access-control/built-in-roles#contributor
With more specific role 
https://docs.microsoft.com/fr-fr/azure/role-based-access-control/built-in-roles#classic-virtual-machine-contributor

Assigment to a scope is done afterwards.

We can also create an application in Azure to leverage active directory on own application <!-- did not explore this feature -->

## Implement Azure access and governance tools 

(ps 4 OK)

### Azure policy

- Azure tags: k/v par assigned to Azurs
    - can be enforced by policy 
    - used 
        - enforce sec req
        - control cost
            - Tag on specific deparment / projects 
            - rules on cost report to close ressource down
            - to deploy software on machine with specific tag

- Az policiy is connection of rules assigned to a scope
Usage is 
    - policy definition 
    - policy assignment ( a ressource, a subscription)
    - policy parameters


- initiaves are collection of policies, with same usage as policy

- We have built-in policies (and create custompolivy from there)

=> By combining RBAC scoped to RG + policy assigned to RG we can linit what a user an do

### Azure blueprints

- Ochestrate deployment of ressource templates 
- Maintain relationship with deployed resource
- Includes Azure policy +  initiative 
- Includes roles

We can 
- create resource group
- deploy resource via ARM templates
- Include Azure policy to enforce compliance
- Assign roles to resources that blueprint have created

See https://docs.microsoft.com/en-us/azure/governance/blueprints/overview 

## Azure security options 

(ps 5+6 OK)

PS azure sec/part 6/mod3/1'14

### Options are 

- Azure firewall 
From  https://docs.microsoft.com/en-us/azure/firewall/overview

> Azure Firewall is a cloud-native and intelligent network firewall security service that provides the best of breed threat protection for your cloud workloads running in Azure. It's a fully stateful, firewall as a service with built-in high availability and unrestricted cloud scalability. It provides both east-west and north-south traffic inspection.

- Azure DDoS protection

https://docs.microsoft.com/en-us/azure/ddos-protection/ddos-protection-overview

- Azure Web application firewall (on Azure application gateway)

From https://docs.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview

> Azure Web Application Firewall (WAF) on Azure Application Gateway provides centralized protection of your web applications from common exploits and vulnerabilities. Web applications are increasingly targeted by malicious attacks that exploit commonly known vulnerabilities. SQL injection and cross-site scripting are among the most common attacks.

See APIM integration in Pluralsigth APIM course: https://app.pluralsight.com/library/courses/microsoft-azure-api-management-strategy-designing/table-of-contents and related to [Azure APIM notes](Azure-APIM-k8s-Kafkapubsub-and-DAPR-integration.md).


- Network security group

From https://docs.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview

> You can use an Azure network security group to filter network traffic to and from Azure resources in an Azure virtual network. A network security group contains security rules that allow or deny inbound network traffic to, or outbound network traffic from, several types of Azure resources. For each rule, you can specify source and destination, port, and protocol.

We can use application security group: https://docs.microsoft.com/en-us/azure/virtual-network/application-security-groups 

> Application security groups enable you to configure network security as a natural extension of an application's structure, allowing you to group virtual machines and define network security policies based on those groups. You can reuse your security policy at scale without manual maintenance of explicit IP addresses. The platform handles the complexity of explicit IP addresses and multiple rule sets, allowing you to focus on your business logic. 

- Forced tunneling  
    - Via VPN
    - Via Express route

See VPN reminder [here](Networking/basic.md).

From https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm


> Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a Site-to-Site VPN tunnel for inspection and auditing. This is a critical security requirement for most enterprise IT policies. If you don't configure forced tunneling, Internet-bound traffic from your VMs in Azure always traverses from the Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic. Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.
> Forced tunneling in Azure is configured using virtual network **custom user-defined routes.** Redirecting traffic to an on-premises site is expressed as a Default Route to the Azure VPN gateway.
> ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via the ExpressRoute BGP peering sessions. For more information, see the ExpressRoute Documentation.

- Marketplaces devices
    - or setup your appliance by yourself (just use the IaaS)

### Some use-cases

- Control flow of traffic heading to internet at layer 7: User defined route (UDR)to Azure firewall or marketplace devices (own appliance)
- Limit traffic fron Azure subnet to be allowed to Access Azure SQL server: NSG
- All internet traffic that is generated by AS nust be routed though HQ (firewall on premise): Forced tunneling


### Demo in PS do UDR + Azure firewall

Here are explained UDR:
https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview
And how to do routing via UDR to firewall
https://docs.microsoft.com/en-us/azure/firewall/tutorial-firewall-deploy-portal
> Step 8: For Next hop type, select Virtual appliance.
> Azure Firewall is actually a managed service, but virtual appliance works in this situation.


## Azure securiy and report tool 

(ps7 todo)

## Azue compliance and data protection standard 

(ps8 todo)