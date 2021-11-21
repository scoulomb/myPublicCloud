# Platform solutions
<!-- 1,2,3,4 concluded --> 

We will take example of Azure.

Ressource: 
- https://app.pluralsight.com/library/courses/microsoft-azure-services-concepts/table-of-contents
- AZ900 book, chapter 3 (skills 3.1 describe core solutions available in Azure)

## Big Data

### Azure Synapse

From AZ900 book, p105
4 differents components
- Synapse SQL (data warehousing option)
- Apache Spark integration 
- Data integration of Spark and Azure Data Lake Storage
- a web-based user interface called Azure Synapse Studio


Queries are executed in compute nodes.
Data is stored in a built-in SQL serverless pool (it will not appear in your "all ressources" list, we have a synapse workspace and storage account )
=>  Similar to [serverless tier](3-cloud-db-overview.md#Azure-SQL-database).

From Synapse wokrspace (ressource) We can create 
- new dedicated SQL pool: in that case it will appear as a SQL dedicated pool in all ressource. It is a product for Synapse Analystics
https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is
=>  Similar to [provisionned tier](3-cloud-db-overview.md#Azure-SQL-database)
-  new Apache Spark pool: note increation process we select number of nodes (no servereless).
The pool does actually in all ressource.
We can scale after number of node in the pool but we can not access to VM.
=> [PaaS with node farm but no @ccess](1-cloud_iass-pass-saas.md#From-IaaS-to-diffferent-level-of-PaaS).
- New Apache explore pool

![Resource synapse analytics](doc/ressource-synapse-analytics.png).

Thus we have new ressource created leveraged by Synapse but no direct access to underlying VMs.

### HDInsigth 

Azure managed service (cloud implem) of open source service (Hadoop).
It supports cluster of type:
- Hadoop
- HBase
- Storm
- Spark 
- Interactive Query (Hive)
- RServer
- Kafka

### Databrick

Cloud implem of Spark.
It includes [Spark notebook](https://github.com/spark-notebook/spark-notebook) (similar to Jupyter).


AZ900 book, page 113
> Databrick uses a serverless miodel if computing. That means that when you are not running any jobs, you don't have any VMs or compute ressource assigned to you. When you run ajob, Azure will allocate VMs to your cluster temporarily in order to process that job. Once the job is compltete, it releases those resources.
=> [Serverless](1-cloud_iass-pass-saas.md#From-IaaS-to-diffferent-level-of-PaaS).

<!-- 3  questions resolved + big data concluded -->
<!-- all above ok -->
<!-- Azure function concluded and juge consistent OK RECCL AND YES => OK INDEED STOP HERE - OK CCL CONFIRMED - STOP SUPER CLEAR - and linked below in Azure function with double pointer to iaas to pass OK-->

## Cognitive service 

It is a SaaS ML (p119) => SaaS or PaaS for service below ask ourself question (where service in [big data](#Big-Data) are PaaS, even if Azure Synapse studio)
<!-- in 2/aws/ dynamodb saas in ps ok, stop here -->
See doc here: https://azure.microsoft.com/fr-ca/services/cognitive-services/#overview

## Machine learning

AZ900 p 118.

https://docs.microsoft.com/en-us/azure/machine-learning/overview-what-is-azure-machine-learning

> Azure Machine Learning is a cloud service for accelerating and managing the machine learning project lifecycle. Machine learning professionals, data scientists, and engineers can use it in their day-to-day workflows: Train and deploy models, and manage MLOps.

> You can create a model in Azure Machine Learning or use a model built from an open-source platform, such as Pytorch, TensorFlow, or scikit-learn. MLOps tools help you monitor, retrain, and redeploy models.

It is a [SaaS](https://www.logiciels.pro/logiciel-saas/microsoft-azure-machine-learning).

## Bot service 

Between PaaS and SaaS

## Iot


- IoT Hub: PaaS offering to connecti millions of device securely and at scale
- IoT Central: SaaS offering to connect, manage and monitor devices at scale
 
See https://stackoverflow.com/questions/56425557/what-is-the-difference-between-azure-iot-hub-and-azure-iot-central

- Azure Sphere: are services and products from Microsoft that allows vendors of Internet of Things devices to increase security by combining a specific system on a chip, Azure Sphere OS and an Azure-based cloud environment for continuous monitoring. 

See https://en.wikipedia.org/wiki/Azure_Sphere


## Serverless (PaaS)

(AZ900 book, p122)
<!-- serverless ccl, deja ccl ok, pas revenu-->
- Azure function, logic apps and event grid
- For Azure function, see [2-cloud-compute-overview](2-cloud-compute-overview.md#Azure-function)
- For integration see [2-cloud-compute-overview](2-cloud-compute-overview.md#Example-of-Azure-serverless-integration-with-function-and-event-grid).
  

## DevOps solutions (separated from solution in pluralsigth)

- Azure devops: https://azure.microsoft.com/fr-fr/services/devops/#overview
    - Azure boards
    - Azure pipleine 
    - Aure repos
    - Azure test plans: create abd track test to ensure reliable software release (AZ900 book, p139)
    - Azure artifacts
    - + Market place
- Azure devtest labs: Quickly create environments using reusable templates and artifacts
    - Quickly provision development and test environments
    - Minimise waste with quotas and policies
    - Set automated shutdowns to minimise costs
    - Build Windows and Linux environment
- github tools: https://github.com/features/actions


In core solution we have seen solution which are not 
- compute (VMs)
- DB or datastorage 

In compute we had seen Azure function, event grid and logic apps.
Only function is compute. Cf [note](2-cloud-compute-overview.md#Logic-app-is-not-really-a-compute-service).

<!-- all above ok ccl ok, consistent az900 books skills 3.1 ==> p 89-151 - STOP HERE, NO CHECK ABOVE CONCLUDED -->

[here]
