# DB

## Azure

### Deploy db virtual machine image by yourself (IaaS)

### Microsft SQL server options: https://portal.azure.com/#create/Microsoft.AzureSQL

- SQL on VM (IaaS)
This is considered as IaaS and not a PaaS
Actually this is an helper to depploy SQL image on VM.

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

- Azure SQL server (PaaS)

SQL server can create sql database within, we have no vm access (confirmed with test)

- Azure SQL manage instance (PaaS) - not avail in free tier but same no access to VM

See https://medium.com/awesome-azure/azure-difference-between-azure-sql-database-and-azure-sql-managed-instance-sql-mi-2e61e4485a65

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
https://aws.amazon.com/blogs/database/getting-started-with-amazon-rds-managed-databases-in-your-on-premises-vmware-environment/
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
| PaaS with @ to managed nodes| None (except AWS RDS and VMWare)                                             |
| PaaS with node farm but no @| **Azure (MS) SQL server**, **Azure (MS) SQL manageed instance**, **Azure db for pgSQL**, **Azure database for MariaDB**, RDS with [Aurora, pgSQL, mySQLm MariaDB, Oracle, MS SQL]|                                         
| Serverless                  | **Azure Cosmo DB**, **Azure cache for Redis**, Aurora serverless             |
| FaaS                        | N/A                                                                   |                                    

- Note Azure db in "PaaS with node farm but no @", we mention server size unlike Azure serverless db (Azure Cosmos DB)


[we are here remaing is clear !]
# File storage

## Azure 

### other storage

Blob ...
