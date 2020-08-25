# Cloud computing

From: https://fr.wikipedia.org/wiki/Cloud_computing

Le cloud computing en français l'informatique en nuage, correspond à l’accès à des services informatiques (serveurs, stockage, mise en réseau, logiciels) 
via Internet (le « cloud » ou « nuage ») à partir d’un fournisseur.


Advantages listed here: https://en.wikipedia.org/wiki/Cloud_computing, my favourite one with my inputs

- No CAPEX, only OPEX (no initila cost, can pay when make money)
- Scalability and elasticity (+serverless later -> pay what you actually really use/need)
- Locality (for faster response or legal reasons)
- Availability: use of multiple redundant sites, which makes well-designed cloud computing suitable for business continuity and disaster recovery.
- Security: centralize security effort 
- multitenancy: share resource and cost across several users -> centralization of infrastructure in locations with lower costs (such as real estate, electricity, etc.)

<!-- aws prep step 2, => question completing this response-->

# Microservices on AWS Whitepaper
 
Though from: https://d0.awsstatic.com/whitepapers/microservices-on-aws.pdf

## Clarification on serverless 

### Source 

- https://youtu.be/IEvLkwdFgnU?t=372
- https://aws.amazon.com/blogs/aws/amazon-eks-on-aws-fargate-now-generally-available/


### Compare

| Layer        | AWS ECS             | AWS EKS             | AWS Lambda        | On premise (RedHat)  | Not using a managed k8s  | 
| --------     | -----------         | ----------------    | ----------------- | -----------------    | ----------------- 
| SAAS         | My service          | My service          | My service        | My Service           | My service
| PAAS         | ECS                 | EKS                 | AWS Lamdba        | OpenShift (k8s)      | OpenShift/k8s + custom
| IAAS         | EC2  or **Fargate** | EC2  or **Fargate** | **Abstracted**    | OpenStack /vio       | EC2
                                                                                                                                                                                 
Image registry ECS, Dockerhub

Same with GCP/Azure

PAAS is a CAAS when not lambda
Also k8s is not a all inclusive PAAS (https://en.wikipedia.org/wiki/Platform_as_a_service);
> Kubernetes is not a traditional, all-inclusive PaaS (Platform as a Service) system. Since Kubernetes operates at the container level rather than at the hardware level, it provides some generally applicable features common to PaaS offerings, such as deployment, scaling, load balancing, and lets users integrate their logging, monitoring, and alerting solutions. However, Kubernetes is not monolithic, and these default solutions are optional and pluggable. Kubernetes provides the building blocks for building developer platforms, but preserves user choice and flexibility where it is important.


### Serverless cases

Case in bold are serverless
- ECS with Fargate
- EKS with Fargate 
- Lambda


When using EC2, we are billed EC2 instance .
We have elasticity with EC2 server but:
- I have to pre-provision ec2 instance in advance (though done automatically)
- I pre-purchased a given amount of capacity

Still true even if we have quota in the cluster:
1. [Resource quotas across multiple projects](https://docs.openshift.com/container-platform/4.5/applications/quotas/quotas-setting-across-multiple-projects.html) (this is an OpenShift concept),
1. [ns/project resource quota](https://kubernetes.io/docs/concepts/policy/resource-quotas/),
1. and [LimitRange](https://kubernetes.io/docs/concepts/policy/limit-range/) which is a policy to constrain resource allocations 
(to Pods or Containers) in a namespace.).

Note 3 is in 2 which is in 1. 

<!-- SRE setup related ok, suffit -->

What If I would pay my actual consumption at appli/Docker level and not at VM provisionning.
(serverless)

https://d1.awsstatic.com/events/reinvent/2019/NEW_LAUNCH_REPEAT_1_Running_Kubernetes_Applications_on_AWS_Fargate_CON326-R1.pdf

From Wikipedia: https://en.wikipedia.org/wiki/Serverless_computing
>Serverless computing is a cloud computing execution model in which the cloud provider runs the server, and dynamically manages the allocation of machine resources. 
> **Pricing is based on the actual amount of resources consumed by an application, rather than on pre-purchased units of capacity.**
> It can be a form of utility computing.
> Serverless computing can simplify the process of deploying code into production. Scaling, capacity planning and maintenance operations may be hidden from the developer or operator. Serverless code can be used in conjunction with code deployed in traditional styles, such as microservices. Alternatively, applications can be written to be purely serverless and use no provisioned servers at all.[2] 

We do not need to provision a cluster + pay as you actually use.

### Function As A Serivce (FAAS)

See AWS lambda layer.
PAAS layer becomes a FAAS layer.

Azure function <=> AWS lambda
Opensource for serverless (OpenFasS) => https://docs.openfaas.com/deployment/kubernetes/
We can try on Minikube: https://medium.com/faun/getting-started-with-openfaas-on-minikube-634502c7acdf

| Layer        | OpenFaaS (cluster admin vision)    | OpenFass (user vision)
| --------     | -----------                        | ----------------    
| SAAS         | OpenFass                           | MyService
| PAAS         | OpenShift                          | OpenFasS
| IAAS         | OpenStack                          | Abstracted


If we deploy OpenFaaS (on premise or EKS as in OpenFass doc) but alone to use it, no big benefit (as it would cost more to run the OpenFaaS server than the appli)
Here it shows the benefits of using AWS lambda.

Note OpenFass can trigger Python, but even Docker image.

If I would do an Ansible playbook triggering an OpenShift job (docker image).
It would look like an OpenFaas Implem where a Docker is runned. (Ansible tower is playing the role as OpenFaas framework as running in the PAAS)

Here https://github.com/scoulomb/soapui-docker/blob/master/presentation.md#alternative-and-perspective:I propose a CronJob tu run non reg rather than having a permanent Jenkins server.
This would be useful if running on Fargate.

Note we also have OpenShift serverless operator:
- https://docs.openshift.com/container-platform/4.5/serverless/installing_serverless/installing-openshift-serverless.html
= https://www.openshift.com/learn/topics/serverless

## Other

- https://cloud.google.com/kubernetes-engine/docs/concepts/service-discovery

- https://www.redhat.com/fr/topics/microservices/what-is-a-service-mesh
=> istio => https://istio.io/latest/docs/setup/getting-started/

- SAGA pattern could be interesting. 
<!-- dns, puis fw, puis lb -->

- step function close to Conductor
https://aws.amazon.com/fr/step-functions/
<!-- mae -->

- SAGA implem 
    - Step function + SAGA: https://theburningmonk.com/2017/07/applying-the-saga-pattern-with-aws-lambda-and-step-functions
    - conductor + SAGA: https://www.ben-morris.com/events-sagas-and-workflows-managing-long-running-processes-between-services/

