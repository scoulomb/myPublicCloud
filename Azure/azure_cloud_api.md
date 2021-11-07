# Azure API 


## General 

Some ressources:

- Azure service course: https://app.pluralsight.com/library/courses/microsoft-azure-services-concepts/exercise-files
- understanding-azure-architecture-and-management-slides

<!--
Slide 22. 2/azure source manager
confirm left is ARM API
--> 

Azure Resource Manager REST API (oauth2), Azure SDKs, Azure portal, Azure Powershell, Azure CLI, Azure cloud Shell (via Azure powershell or cli)

Targets Azure RESSOUCE MANAGER

Which creates ressources (webapp,vm, db....)

Actually every client uses Azure API behind:https://stackoverflow.com/questions/49291889/does-the-azure-cli-use-the-azure-rest-api


See example of  policy application via API: https://github.com/dapr/samples/tree/master/dapr-apim-integration#echo-service-policy


Also cf. 
https://docs.microsoft.com/en-us/learn/modules/tour-azure-portal/2-azure-management




Note with Azure powershell in cloud shell, linux specific functioaly is availalbe unlile widows specific because cloud shell runs poershell on a linux container (esi questions)


## Arm template

We can upload an ARM template portal via rest API, powershell and azure cli but also

- From github: https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-github-actions
- Azure pipline CI/CD : https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/add-template-to-azure-pipelines (deploy task or a task whch run as Azure powersell script)

<!--
slide 25
-->

## Azure blueprint


https://docs.microsoft.com/en-us/azure/governance/blueprints/overview


> The service is designed to help with environment setup. This setup often consists of a set of resource groups, policies, role assignments, and ARM template deployments. A blueprint is a package to bring each of these artifact types together and allow you to compose and version that package, including through a continuous integration and continuous delivery (CI/CD) pipeline. Ultimately, each is assigned to a subscription in a single operation that can be audited and tracked.

> There's no need to choose between an ARM template and a blueprint. Each blueprint can consist of zero or more ARM template artifacts. This support means that previous efforts to develop and maintain a library of ARM templates are reusable in Azure Blueprint



## Clean-up all resource

Azure cloud shell with powershell
https://techgenix.com/removing-azure-resources/

````
Get-AzResource | ForEach { Remove-AzResource -ResourceId $_.ResourceId -Force -Confirm:$False }
````