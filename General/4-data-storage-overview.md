# Data storage

## Azure 


https://portal.azure.com/#blade/Microsoft_Azure_Resources/QuickstartAnchorServicesBlade/goalId/store-backup-archive-data

5 services managed instorage acciybr

- Blob storage: unstructured data
    - Block blob
    - Append blob (for instance logs)
    - page blobs (VM disks and database are using page blob as persistent storage) 
- File storage: same as blob storage but supports SMB protcol so it can be attached to several VMs (network drive later)
    - Good for migration scenarios
    - Can be shared on Azure VM or on premises
- Disk: virtual machine disk
- Table storage: no-sql db (like Cosmos db)
- Queue storage: for service delivery guarantee

LRS storage (locally redudant storage, 3 time in az), and GRS (georedudant) if option to store in several regions.
We can have read access only GRS.

3 access tiers:
    - Hot tier (highest storage cost , lowest data access cost)
    - Cool tier (lower storage cost , higher data access cost)
    - Archive tier (lowest storage cost , highest data access cost)

Support static website hosting and can integrate with CDN.
It intgrates Azure serach.

In Azure Storage Explore desktop app we can see a subscription can contain storage account. A storage account can contain blob container, file share ,tables and queues. Same label can be found when creating s storage account manually.

A subscription can also contain disk.

Using Azure Storage explorer and creating a microsoft disk it enables to see the disk 
We have subscprition
    - Storage account 
        - container blobs
    - Disks
        - Azure disks

So disks does not appear as a page blob.

We actually created a manage disk (cf AZ900 book).
See https://aidanfinn.com/?p=20441 where we create an umanaged disk from a blob.

<!-- both mamnaged and umanaged available in classic/non classic version of disk-->

I created a Cosmos db and it is not shown as a storage resource (anywhere) even in cosmos db deprecated tab.

For large data we can use Azure Data Box.

### AWS and MS Azure storage 

Some doc

- https://aws.amazon.com/products/storage/
- https://aws.amazon.com/fr/fsx/windows/ (fsx is also SMB proto)
- https://docs.microsoft.com/en-us/azure/storage/queues/storage-queues-introduction
- https://aws.amazon.com/sqs/

| Azure techno                | AWS equivalent                                                               |
| --------------------------- | --------------------------------------------------------------------------   |
| Blob                        | Amazon Simple Storage Service (S3)                                           |
| File storage                | Amazon Elastic File System (EFS), Amazon FSx                                 |
| Disk                        | Amazon Elastic Block Store (EBS)                                             |
| Queue                       | Amazon SQS (Simple queue storage)                                            |
| Table (CosmosDB)            | DynamoDB                                                                     |

Also
- AWS S3 Infrequent access <=> Azure Storage cool tier
- AWS S3 Glacier, Deep archive <=> Azure Storage archive access tier

And Azure Databox <=> snowball and snowmobile

See also https://docs.microsoft.com/en-us/azure/architecture/aws-professional/services
<!-- AZ990 book consistent ok -->

When we create a cloud shell we can see the storage account in explorer.

[Next: platform solutions](5-platform-solutions.md)