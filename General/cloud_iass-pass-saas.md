# Cloud compution IaaS,PaaS, SaaS


## Basic definitition

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

We can consider here Azure AKS, AWS EKS.
Note we have access to ec2 instance when not [using Fargate](#AWS_Fargate) (see below)
https://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance-connect.html
https://docs.aws.amazon.com/eks/latest/userguide/eks-compute.html

Therefore we can see we have to upgrade more or less AMI 
> Yes – If you deployed an Amazon EKS optimized AMI, then you're notified in the Amazon EKS console when updates are available and can perform the update with one click in the console. If you deployed a custom AMI, then you're not notified in the Amazon EKS console when updates are available and must perform the update yourself

Thus not fully a PaaS on that aspect also.



For pure container engine we have AWS ECS (https://aws.amazon.com/ecs/?nc1=h_ls), Azure container instance (https://azure.microsoft.com/en-us/services/container-instances/#overview), Google cloud run

We have PaaS with Docker support cf. AWS Elastic Beanstalk with docker (https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.html)



## FaaS, serverless

https://en.wikipedia.org/wiki/Function_as_a_service

> Platform as a service (PaaS) application hosting services are similar to FaaS in that they also hide "servers" from developers. However, such hosting services typically always have at least one server process running that receives external requests. Scaling is achieved by booting up more server processes, which the developer is typically charged directly for. Consequently, scalability remains visible to the developer.[5]

> By contrast, FaaS does not require any server process constantly being run. While an initial request may take longer to be handled than an application hosting platform (up to several seconds[6]), caching may enable subsequent requests to be handled within milliseconds. As developers only pay for function execution time (and no process idle time), lower costs at higher scalability can be achieved (at the cost of latency). 

Here we can find Azure function, AWS lambda

Google cloud run is a serverless CaaS: https://cloud.google.com/serverless, https://wiki.sfeir.com/googlecloudplatform/compute/cloud-run/

## AWS Fargate

We can find AWS ECS and AWS EKS with FARGATE.
See
- https://aws.amazon.com/about-aws/whats-new/2019/12/run-serverless-kubernetes-pods-using-amazon-eks-and-aws-fargate/?nc1=h_ls
- https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html

Unlike AWS ECS and EKS without Fargate we have no access to EC2 instance behind.

Fargate is just less, not serverless (closer to PaaS)

From https://www.trek10.com/blog/is-fargate-serverless
> Don’t let the word “serverless” confuse you: Fargate is fundamentally just less infrastructure; Lambda-based “serverless” confers benefits to your business on a scale that Fargate simply can’t touch.


## DBaaS

Plurasight
When we deploy RDS we do not have access to EC2 behind (see example). RDS is PaaS
They consider dynamoDB as a SaaS as it is exposed via API or CLI. (ask question, PS aws cloudfoundation tra)

MongoDB says the following (closer to SaaS)

> Is DBaaS a Platform as a Service (PaaS)?
> Platform as a Service provides a platform for their customers to run their business application on without the need to build and maintain infrastructure that is typically required by a software development process.
> DBaaS is not a PaaS. The cloud database sits on top of the platform at the application layer. When using a DBaaS service, the costs of the underlying platform are included in the subscription. 

## Company PaaS

We can deploy OpenShift (or build platform on top with logging, monitoring, esb)
Use IaaS feature of cloud provider and offer PaaS service to internal customer.
We can also use a private cloud for this.

## Links

AWS service overview: https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html 


OK: only remain point on dbaas (optional)

## A note on Azure compute service

Azure service course: https://app.pluralsight.com/library/courses/microsoft-azure-services-concepts/exercise-files
Exploring Azure core product

Azure compute has (slide 3)
- Virtual machine ==> IAAS
- Containers ==>  CAAS (ACI (serverless CAAS), AKS)
- Azure App Service ==> PAAS, they have a non serverless CAAS feature: https://azure.microsoft.com/en-us/services/app-service/containers/#demo
- Serverless computing ==> FaaS (functions, logic apps)

So container can be run in 
- local workstation
- on premise server
- VM in Azure
- ACI 
- AKS
- Azure APP service

and it can be kube/ docker compose in first 3 options

For database note (from esi practise question) we have

- AZ sql manage instance we can access VM
https://docs.microsoft.com/en-us/azure/azure-sql/managed-instance/instance-create-quickstart
- unlike azure sql db (serverless)
- and could have Azure VM with a database template (as shown in Az service course)

## own serverless with kubenertes

- OpenFasS
- Launch a docker via job API 

## other notes

See also https://github.com/scoulomb/myPublicCloud/blob/master/AWS/1-microservice-on-aws-notes.md (checked and accurate)

Aure vm is pay as you go