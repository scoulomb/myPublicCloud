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

> **Serverless computing is a cloud computing execution model in which the cloud provider allocates machine resources on demand, taking care of the servers on behalf of their customers.** "Serverless" is a misnomer in the sense that servers are still used by cloud service providers to execute code for developers. However, developers of serverless applications are not concerned with capacity planning, configuration, management, maintenance, fault tolerance, or scaling of containers, VMs, or physical servers. Serverless computing does not hold resources in volatile memory; computing is rather done in short bursts with the results persisted to storage. **When an app is not in use, there are no computing resources allocated to the app**. Pricing is based on the actual amount of resources consumed by an application.[1] It can be a form of utility computing. 

https://www.bmc.com/blogs/serverless-faas/

> As the name suggests, serverless is a computing model where infrastructure orchestration is managed by service providers.

From AZ900 book, page 122.
> Serverless doesn't mean that no VMs are involved, It simplify means that the VM that's running your code isn't explivicylu allocated to you.
> You code is moved to the VM, it's executed, and then it's moved off.

Serverless actually allows cloud provider to monetize unused VM.

Example of serverless in Azure are cognitive service, functions, logic apps (serverless wokflows), event grids.

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
    - Traditionnal PaaS: user can access to "managed" nodes and scaling is made by spinning up more servers (and customers is billed based on #servers used). Scalability is visible to developer. We have 2 options:
        - PaaS with @ccess to managed (dedicated) nodes
        - PaaS with (dedicated) node farm but no @ccess
    - Serverless:
      	- User does not have  access / visibility to  server. And User does not have **Dedicated server.** **Machine are allocated on demand**. They are managed by service provider on behalf of customers (hidden scalalbilty).
        - When **app is not used, no ressouce is allocated to application.**
    - FaaS: If offers possibility for dev to **create software function in the cloud with event driven computing**. It does not require server to constantly run, we pay for function execution time

We could say FaaS is a subset of serverless as when using FaaS, provider manage infra without dedicated server and no ressource alloated when not used. And also oppose them.
Why?:
- https://www.trek10.com/blog/is-fargate-serverless 
- https://www.bmc.com/blogs/serverless-faas/
- https://aws.amazon.com/lambda/ (lamdba is canonical example of FaaS)
> AWS Lambda is a serverless, event-driven compute service that lets you run code for virtually any type of application or backend service without provisioning or managing servers.
- Fargate is serverless and Lamdba is serverless FaaS

For Azure function, see [2-cloud-compute-overview](2-cloud-compute-overview.md#Azure-function)

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


[Next: Cloud compute overview](2-cloud-compute-overview.md)