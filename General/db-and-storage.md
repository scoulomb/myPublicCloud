# DB

## Azure

### Deploy db virtual machine image by yourself (IaaS)

### Microsft SQL server options: https://portal.azure.com/#create/Microsoft.AzureSQL

#### SQL on VM (IaaS)

This is considered as IaaS and not a PaaS
Actually this is an helper to deploy SQL image on VM.

There is no sql ressource like we had for AKS (AKS resource + VM scale set).

So it is a IaaS (and not a PaaS with access to VM).

And Microsft considers as such https://azure.microsoft.com/en-us/services/virtual-machines/sql-server/#faq
https://docs.microsoft.com/en-us/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overvi

Then we manage VM by ourself

Note here we have a IaaS connecor which offers some mamagement capapilities close to a PaaS
https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines
> Automatic SQL Server VM registration
> SQL virtual machine
> This feature will discover and register both existing and future Azure VMs running SQL Server 2008+ in the selected subscription with SQL IaaS Agent Extension. All VMs will default to lightweight mode. This process does not restart the Azure VM or the SQL Server service. 
> To activate full mode features, VMs must be manually upgraded from light weight to full mode. Learn more about SQL IaaS Agent Ext

VMs are visible in virtual machine panel

#### Azure SQL databases

There are 3 deployment options:


- Single database
    - We can pay based on 
        - [DTU model](https://docs.microsoft.com/en-us/azure/azure-sql/database/service-tiers-dtu) 
        > Service tiers in the DTU-based purchase model are differentiated by a range of compute sizes with a fixed amount of included storage, fixed retention period for backups, and fixed price. All service tiers in the DTU-based purchase model provide flexibility of changing compute sizes with minimal downtime; however, there is a switch over period where connectivity is lost to the database for a short amount of time, which can be mitigated using retry logic. Single databases and elastic pools are billed hourly based on service tier and compute size.
        -  vcore model with 
            - [a provisionned](https://docs.microsoft.com/en-us/azure/azure-sql/database/service-tiers-sql-database-vcore): Main differnce with DTU is that we can customize config and not take a pre-exisiting package of RAM, CPU and storage 
            - [serverless tier](https://docs.microsoft.com/en-us/azure/azure-sql/database/serverless-tier-overview)
            > Serverless is a compute tier for single databases in Azure SQL Database that automatically scales compute based on workload demand and bills for the amount of compute used per second.
- elastic pool: From portal description it "provide a cost-effective solution for managing the performance of multiple databases with variable usage pattern". See also https://beingadba.com/2020/05/25/single-database-vs-elastic-pool/. It is actually a way to have several DB sharing same pool of resource which do not have peak at same time
- Database server: "database servers are used to manage groups of single databases and elastic pools. 
- Actually a single db database is part of
    - a server [deployment option 1]
    - an elastic pool which is part of a server [deployment option 2] 
    Thus an elastic pool has same pricing startegy as single database.

we have no vm access (confirmed with test)

Note from a single database editor of db1 we can run (tested without elastic pool)

````
CREATE DATABASE database_name;
````
This will create a new single database  (new azure ressource) attached on that db server.

#### Azure SQL manage instance (PaaS) [deployment option 3]

Recommend for lift and shift.

We have 2 option

- Single instance (not avail in free tier, tested with pas-as-you go subscription). No access to VM. It will deploy a SQL managed instance, nsg, route table and vnet.

- Single instance - Azure Arc: it deploy a manage instance on your own VM/k8s!": https://docs.microsoft.com/en-us/azure/azure-arc/data/managed-instance-overview



Note from [DTU model](https://docs.microsoft.com/en-us/azure/azure-sql/database/service-tiers-dtu) 
> Azure SQL Managed Instance does not support a DTU-based purchasing model.

We have access to a managed server and can create database from there. Cf
    - https://docs.microsoft.com/en-us/azure/azure-sql/managed-instance/connect-vm-instance-configure
    - https://docs.microsoft.com/fr-fr/sql/t-sql/statements/create-database-transact-sql?view=sql-server-ver15&tabs=sqlpool


### Other databases

https://azure.microsoft.com/en-gb/product-categories/databases/
Our test show we have no access to VM


- Azure Database for PostgreSQL: https://portal.azure.com/#create/Microsoft.PostgreSQLServer
    - Used single server (west us) for test
- Azure Database for MySQL: https://portal.azure.com/#create/Microsoft.MySQLServer
    - Used single server (west us)
- Azure Database for MariaDB: https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DBforMariaDB%2Fservers
    -Use west us
- Azure Cosmos DB: https://portal.azure.com/#create/Microsoft.DocumentDB
    - Used Core (SQL) - Recommended
- Azure Cache for Redis
https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Cache%2FRedis


## AWS 

- Deploy DB virtual machine image by yourself on EC2 (IaaS)
- Use Amazon EC2 for SQL server (IaaS)
    - https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-sql-server/ec2-sql.html
    - https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/migrate-an-on-premises-microsoft-sql-server-database-to-amazon-ec2.html?did=pg_card&trk=pg_card
- Deploy EC2 with SQL (IaaS)
From https://app.pluralsight.com/library/courses/understanding-aws-core-services/table-of-contents 
- RDS: https://aws.amazon.com/rds/?nc1=h_ls (PaaS)
It suports  several databases engines
    - Aurora
    - PostgreSQL
    - MySQL
    - Maria DB
    - Oracle 
    - MS SQL server

From my test with RDS we do not have access to managed node (EC2 instance, cf. also ps Tucker/ understanding aws core svc /  module 6 db / summary / scenario 2) but can choose the size of the nodes
It is also possible to use RDS on-premise. In that particular cases case we have access to nodes:
https://aws.amazon.com/blogs/database/getting-started-with-amazon-rds-managed-databases-in-your-on-premises-vmware-environment/ (similar to Azure arc option)
- DynamoDB  (PaaS (https://www.section.io/engineering-education/getting-started-with-aws-dynamodb/)
    - Pluralsigh considers it is SaaS as it is serverless cf. Tucker/ understanding aws core svc /  module 6 db / DynamoDB): https://aws.amazon.com/dynamodb/
    - Actually some would argue DBaaS is more SaaS. Cf [cloud_iaas-paas-saas](./cloud_iaas-paas-saas.md#DBaaS).

- elastic search and redis (PaaS)
- We also have Aurora serverless (PaaS)
https://www.lemagit.fr/conseil/Bien-choisir-entre-Amazon-RDS-pour-Aurora-et-Aurora-Serverless

## Summary


| Layer                       | Compute                                                                      |
| --------------------------- | --------------------------------------------------------------------------   |
| IaaS                        | **DB img Azure VM**, DB img AWS EC2, **Azure SQL on VM**, EC2 for SQL server |
| PaaS with @ to managed nodes| Exception of AWS RDS and VMWare, **Azure SQL managed instance with Arc**     |
| PaaS with node farm but no @| **Azure SQL database except vcore, serverless tier**, **Azure SQL managed instance except with Arc**, **Azure db for pgSQL**, **Azure database for MariaDB**, RDS with [Aurora, pgSQL, mySQLm MariaDB, Oracle, MS SQL]|                                         
| Serverless                  | **Azure SQL database, vcore, serverless tier**, **Azure Cosmo DB**, **Azure cache for Redis**, Aurora serverless            |
| FaaS                        | N/A                                                                   |                                    

- Note Azure db in "PaaS with node farm but no @", we mention server size unlike Azure serverless db (Azure Cosmos DB)


<!-- Compliant AZ900 book -->
[we are here yes, above ok!, 11 nov commit 5h25 OK YES, consistent]
# File storage

## Azure 

### other storage

Blob ...
