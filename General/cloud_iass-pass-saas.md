# Cloud computing IaaS,PaaS, SaaS

## Basic definition

- OnSite/On your own
    svc provider/other manage: Null
    we manage: Networking, Storage, Servers, Virtualization, OS, mdw, runtime, data, application

- IaaS: 
    svc provider/other manage: Networking, Storage, Servers, Virtualization
    we manage:    OS, mdw, runtime, data, application
- PaaS: 
    svc provider/other manage: Networking, Storage, Servers, Virtualization, OS, mdw, runtime
    we manage:    data, application
- SaaS: 
    svc provider/other manage: Networking, Storage, Servers, Virtualization, OS, mdw, runtime, data, application
    we manage: Null

Source: https://www.redhat.com/en/topics/cloud-computing/iaas-vs-paas-vs-saas


## FaaS and serverless

### Serverless

https://en.wikipedia.org/wiki/Serverless_computing

> **Serverless computing is a cloud computing execution model in which the cloud provider allocates machine resources on demand, taking care of the servers on behalf of their customers.** "Serverless" is a misnomer in the sense that servers are still used by cloud service providers to execute code for developers. However, developers of serverless applications are not concerned with capacity planning, configuration, management, maintenance, fault tolerance, or scaling of containers, VMs, or physical servers. Serverless computing does not hold resources in volatile memory; computing is rather done in short bursts with the results persisted to storage. When an app is not in use, there are no computing resources allocated to the app. Pricing is based on the actual amount of resources consumed by an application.[1] It can be a form of utility computing. 

https://www.bmc.com/blogs/serverless-faas/

> As the name suggests, serverless is a computing model where infrastructure orchestration is managed by service providers.


### FaaS

https://en.wikipedia.org/wiki/Function_as_a_service

> **Platform as a service (PaaS) application hosting services are similar to FaaS in that they also hide "servers" from developers. However, such hosting services typically always have at least one server process running that receives external requests. Scaling is achieved by booting up more server processes, which the developer is typically charged directly for. Consequently, scalability remains visible to the developer.[5]**

> By contrast, FaaS **does not require any server process constantly being run. While an initial request may take longer to be handled than an application hosting platform** (up to several seconds[6]), caching may enable subsequent requests to be handled within milliseconds. As developers only pay for function execution time (and no process idle time), lower costs at higher scalability can be achieved (at the cost of latency). 

https://www.bmc.com/blogs/serverless-faas/

 > Function as a Service is a relatively newer concept that aims to offer developers the freedom to create software functions in a cloud environment easily. In this method, the developers will still create the application logic, yet the code is executed in stateless compute instances that are managed by the cloud provider. FaaS provides an event-driven computing architecture where functions are triggered by a specific event such as message queues, HTTP requests, etc.


## From IaaS to diffferent level of PaaS

- IaaS provider can propose base image (AMI, AZ VM template) but we have to manage os, mdw, runtime etc by ourself after...
We can also have auto-scaling.
- PaaS provider will manage underlying infrastructure, and hide servers from customer.
    - Traditionnal PaaS: user can access to "managed" nodes and scaling is made by spinning up more servers (and customers is billed based on #servers used)
    - Serverless: User does not have access/visibility to server. They are managed by service provider on behalf of customers. When app is not used, no ressouce is allocated to application.
    - FaaS: If offers possibility for dev to create software function in the cloud with event driven computing. It does not require server to constantly run, we pay for function execution time

## Container Orchestration

**K8s is not a all-inclusive PaaS and OpenShift is PaaS**

### k8s

Quoting https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/

> Kubernetes is not a traditional, all-inclusive PaaS (Platform as a Service) system. Since Kubernetes operates at the container level rather than at the hardware level, it provides some generally applicable features common to PaaS offerings, such as deployment, scaling, load balancing, and lets users integrate their logging, monitoring, and alerting solutions. However, Kubernetes is not monolithic, and these default solutions are optional and pluggable. Kubernetes provides the building blocks for building developer platforms, but preserves user choice and flexibility where it is important.

See also 
https://www.infoq.com/articles/kubernetes-successful-adoption-foundation/


Same apply to managed k8s: Azure AKS, AWS EKS.

### OpenShift

However OpenShift can bee seen as a PaaS
https://azure.microsoft.com/fr-fr/services/openshift/  

See https://azure.microsoft.com/en-us/services/openshift/ 

> Azure Red Hat OpenShift provides highly available, fully managed OpenShift clusters on demand, monitored and operated jointly by Microsoft and Red Hat. Kubernetes is at the core of Red Hat OpenShift. OpenShift brings added-value features to complement Kubernetes, making it a turnkey container platform as a service (PaaS) with a significantly improved developer and operator experience.

we can quote for instance, feature like source 2 image.

See also https://www.redhat.com/en/topics/cloud-computing/iaas-vs-paas-vs-saas where they explicitly mention OpenShift is a PaaS.

See https://cloudowski.com/articles/10-differences-between-openshift-and-kubernetes/

Also OpenShift everything battery included (route, no ingress to install)

## CaaS: Container As A Service

https://www.redhat.com/fr/topics/cloud-computing/what-is-caas

> In the range of cloud computing services, CaaS is considered a kind of subset of Infrastructure-as-a-Service (IaaS) and is found between IaaS and Platform-as-a-Service (PaaS).


## DBaaS


MongoDB says the following (closer to SaaS)

> Is DBaaS a Platform as a Service (PaaS)?
> Platform as a Service provides a platform for their customers to run their business application on without the need to build and maintain infrastructure that is typically required by a software development process.
> DBaaS is not a PaaS. The cloud database sits on top of the platform at the application layer. When using a DBaaS service, the costs of the underlying platform are included in the subscription. 

## Company PaaS

We can deploy OpenShift (or build platform on top with logging, monitoring, esb)
Use IaaS feature of cloud provider and offer PaaS service to internal customer.
We can also use a private cloud for this.


## Cloud provider compute service overview


### Overview 

| Layer                       | Compute                                                                            |
| --------------------------- | -----------------------------------------------------------------------------------|
| IaaS                        | Azure VM, AWS EC2                                                                  |
| PaaS with @ to managed nodes| Azure AKS, AWS EK(8s)S, AWS EC(container)S, Azure App Svc                          |                 
| Serverless                  | Azure ACI (container instance) ,Fargate AWS EKS, Fargate AWS ECS, Google cloud run |
| FaaS                        | Azure function, AWS lambda, Azure logic Apps                                       | 


### Comments

Note several serverless service have a cold start (close to FaaS)

Proof app service is a PaaS with access to mamaged nodes: https://docs.microsoft.com/en-us/azure/app-service/overview
> With App Service, you pay for the Azure compute resources you use. The compute resources you use are determined by the App Service plan that you run your apps on. For more information, see Azure App Service plans overview.
And similarly for elastic bean elasticbeanstalk
https://docs.aws.amazon.com//latest/dg/using-features.ec2connect.html

Proof we have access to ec2 instance when not [using Fargate](#AWS_Fargate) in AWS EKS and ECS
https://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance-connect.html
https://docs.aws.amazon.com/eks/latest/userguide/eks-compute.html

This quotes shows nodes are managed
> Yes – If you deployed an Amazon EKS optimized AMI, then you're notified in the Amazon EKS console when updates are available and can perform the update with one click in the console. If you deployed a custom AMI, then you're not notified in the Amazon EKS console when updates are available and must perform the update yourself

AWS ECS and AWS EKS are available with FARGATE.
- https://aws.amazon.com/about-aws/whats-new/2019/12/run-serverless-kubernetes-pods-using-amazon-eks-and-aws-fargate/?nc1=h_ls
- https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html
Unlike AWS ECS and EKS without Fargate we have no access to EC2 instance behind.

Fargate AWS ECS (https://aws.amazon.com/ecs/?nc1=h_ls) and Azure container instance (https://azure.microsoft.com/en-us/services/container-instances/#overview) and Google cloud run are similar. Those are serverless CaaS.

ACI is serverless https://docs.microsoft.com/en-us/azure/container-instances/ unlke AKS, see https://docs.microsoft.com/fr-fr/azure/aks/ssh 

AWS Elastic Beanstalk(https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.html) and [Azure app service]( https://azure.microsoft.com/en-us/services/app-service/containers/#demo) both supports Docker and are similar services.


Here Fargate vs Lambda ([or serverless vs FaaS](From-IasS-to-serverless-PaaS)): https://www.trek10.com/blog/is-fargate-serverless
> Don’t let the word “serverless” confuse you: Fargate is fundamentally just less infrastructure; Lambda-based “serverless” confers benefits to your business on a scale that Fargate simply can’t touch.


### A note on Azure compute service

Azure service ps course: https://app.pluralsight.com/library/courses/microsoft-azure-services-concepts/exercise-files => Exploring Azure core product

Azure compute has (slide 3)
- Virtual machine ==> IaaS
    - We managed node but can use template image (we will have to manage nodes, patch etc)
    - it is a pay as you go model
- Containers (PaaS, CaaS)
    - AKS (we can access nodes but they are managed by platform including software patch, it is not serverless)
    - ACI (serverless CaaS: we can NOT access nodes and they are managed for us)
        - https://medium.com/asos-techblog/serverless-on-azure-b12bc282304a
        - https://docs.microsoft.com/en-us/azure/container-instances/container-instances-overview
- Azure App Service ==> PAAS,
    - they have a non serverless [CAAS feature]( https://azure.microsoft.com/en-us/services/app-service/containers/#demo)
- Serverless computing ==> FaaS (functions, logic apps)

So container can be run in 
- local workstation
- on premise server
- VM in Azure
- ACI 
- AKS
- Azure APP service

and it can be kube/docker compose in first 3 options


## Own serverless with kubenertes

- OpenFasS
- Launch a docker via job API 


[HERE CONFIRMED OK]

## Cloud provider database service overview 

For database note (from esi practise question) we have

- AZ sql manage instance we can access VM
https://docs.microsoft.com/en-us/azure/azure-sql/managed-instance/instance-create-quickstart
- unlike azure sql db (serverless)
- and could have Azure VM with a database template (as shown in Az service course)


OK: only remain point on dbaas (optional)

Plurasight
When we deploy RDS we do not have access to EC2 behind (see example). RDS is PaaS
They consider dynamoDB as a SaaS as it is exposed via API or CLI. (ask question, PS aws cloudfoundation tra)


https://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance-connect.html

## other notes

- See also https://github.com/scoulomb/myPublicCloud/blob/master/AWS/1-microservice-on-aws-notes.md (checked and accurate)
- AWS service overview: https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html 


https://servian.dev/azure-az-900-exam-preparation-guide-how-to-pass-in-3-days-dabf5534507a 