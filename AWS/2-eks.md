# EKS

## Create AWS free tier account

aws.admin@<my-gandi-domain>.site

## EKS get started

- Follow proc, we choose to use eksctl for the operation

https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

## EKS based on EC2

Here "Create your Amazon EKS cluster and compute", we choose manage nodes

- Update policy
https://stackoverflow.com/questions/60438285/error-getting-availability-zones-when-trying-to-create-eks-cluster

- Go to user add inline policy 
ec2 and eks all 

- Have pub ssh key
ssh keygen to generated ssh key

- Add other inline policy
add iam and cloud-formation

- Create cluster 
````
eksctl create cluster \
--name prod2 \
--version 1.17 \
--region us-west-2 \
--nodegroup-name linux-nodes \
--node-type t3.medium \
--nodes 3 \
--nodes-min 1 \
--nodes-max 4 \
--ssh-access \kubec
--managed

````

````shell script
sylvain@sylvain-hp:~$ eksctl create cluster \
> --name prod2 \
> --version 1.17 \
> --region us-west-2 \
> --nodegroup-name linux-nodes \
> --node-type t3.medium \
> --nodes 3 \
> --nodes-min 1 \
> --nodes-max 4 \
> --ssh-access \
> --managed
[ℹ]  eksctl version 0.25.0
[ℹ]  using region us-west-2
[ℹ]  setting availability zones to [us-west-2a us-west-2c us-west-2b]
[ℹ]  subnets for us-west-2a - public:192.168.0.0/19 private:192.168.96.0/19
[ℹ]  subnets for us-west-2c - public:192.168.32.0/19 private:192.168.128.0/19
[ℹ]  subnets for us-west-2b - public:192.168.64.0/19 private:192.168.160.0/19
[ℹ]  using SSH public key "/home/sylvain/.ssh/id_rsa.pub" as "eksctl-prod2-nodegroup-linux-nodes-1f:a8:7f:b2:c8:bd:50:ec:b9:66:f2:48:f6:45:e8:87" 
[ℹ]  using Kubernetes version 1.17
[ℹ]  creating EKS cluster "prod2" in "us-west-2" region with managed nodes
[ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial managed nodegroup
[ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-west-2 --cluster=prod2'
[ℹ]  CloudWatch logging will not be enabled for cluster "prod2" in "us-west-2"
[ℹ]  you can enable it with 'eksctl utils update-cluster-logging --region=us-west-2 --cluster=prod2'
[ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "prod2" in "us-west-2"
[ℹ]  2 sequential tasks: { create cluster control plane "prod2", 2 sequential sub-tasks: { no tasks, create managed nodegroup "linux-nodes" } }
[ℹ]  building cluster stack "eksctl-prod2-cluster"
[ℹ]  deploying stack "eksctl-prod2-cluster"


[ℹ]  building managed nodegroup stack "eksctl-prod2-nodegroup-linux-nodes"
[ℹ]  deploying stack "eksctl-prod2-nodegroup-linux-nodes"
[ℹ]  waiting for the control plane availability...
[✔]  saved kubeconfig as "/home/sylvain/.kube/config"
[ℹ]  no tasks
[✔]  all EKS cluster resources for "prod2" have been created
[ℹ]  nodegroup "linux-nodes" has 3 node(s)
[ℹ]  node "ip-192-168-34-151.us-west-2.compute.internal" is ready
[ℹ]  node "ip-192-168-6-53.us-west-2.compute.internal" is not ready
[ℹ]  node "ip-192-168-95-66.us-west-2.compute.internal" is ready
[ℹ]  waiting for at least 1 node(s) to become ready in "linux-nodes"
[ℹ]  nodegroup "linux-nodes" has 3 node(s)
[ℹ]  node "ip-192-168-34-151.us-west-2.compute.internal" is ready
[ℹ]  node "ip-192-168-6-53.us-west-2.compute.internal" is not ready
[ℹ]  node "ip-192-168-95-66.us-west-2.compute.internal" is ready
[ℹ]  kubectl command should work with "/home/sylvain/.kube/config", try 'kubectl get nodes'
[✔]  EKS cluster "prod2" in "us-west-2" region is ready
sylvain@sylvain-hp:~$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.100.0.1   <none>        443/TCP   164m
sylvain@sylvain-hp:~$ kubectl get nodes
NAME                                           STATUS   ROLES    AGE    VERSION
ip-192-168-34-151.us-west-2.compute.internal   Ready    <none>   159m   v1.17.9-eks-4c6976
ip-192-168-6-53.us-west-2.compute.internal     Ready    <none>   157m   v1.17.9-eks-4c6976
ip-192-168-95-66.us-west-2.compute.internal    Ready    <none>   157m   v1.17.9-eks-4c6976

sylvain@sylvain-hp:~$ eksctl delete cluster prod2 --region us-west-2 
[ℹ]  eksctl version 0.25.0
[ℹ]  using region us-west-2
[ℹ]  deleting EKS cluster "prod2"
[ℹ]  deleted 0 Fargate profile(s)
[✔]  kubeconfig has been updated
[ℹ]  cleaning up AWS load balancers created by Kubernetes objects of Kind Service or Ingress
[ℹ]  2 sequential tasks: { delete nodegroup "linux-nodes", delete cluster control plane "prod2" [async] }
[ℹ]  will delete stack "eksctl-prod2-nodegroup-linux-nodes"
[ℹ]  waiting for stack "eksctl-prod2-nodegroup-linux-nodes" to get deleted
[ℹ]  will delete stack "eksctl-prod2-cluster"
[✔]  all cluster resources were deleted

````

No cluster on west and east, use only east from now, so here example with default region.

```shell script
eksctl create cluster \
--name prod \
--version 1.17 \
--region us-east-2 \
--nodegroup-name linux-nodes \
--node-type t3.medium \
--nodes 3 \
--nodes-min 1 \
--nodes-max 4 \
--ssh-access \
--managed
```


Output is:

````shell script
sylvain@sylvain-hp:~$ eksctl create cluster \
> --name prod \
> --version 1.17 \
> --region us-east-2 \
> --nodegroup-name linux-nodes \
> --node-type t3.medium \
> --nodes 3 \
> --nodes-min 1 \
> --nodes-max 4 \
> --ssh-access \
> --managed
[ℹ]  eksctl version 0.25.0
[ℹ]  using region us-east-2
[ℹ]  setting availability zones to [us-east-2b us-east-2a us-east-2c]
[ℹ]  subnets for us-east-2b - public:192.168.0.0/19 private:192.168.96.0/19
[ℹ]  subnets for us-east-2a - public:192.168.32.0/19 private:192.168.128.0/19
[ℹ]  subnets for us-east-2c - public:192.168.64.0/19 private:192.168.160.0/19
[ℹ]  using SSH public key "/home/sylvain/.ssh/id_rsa.pub" as "eksctl-prod-nodegroup-linux-nodes-1f:a8:7f:b2:c8:bd:50:ec:b9:66:f2:48:f6:45:e8:87" 
[ℹ]  using Kubernetes version 1.17
[ℹ]  creating EKS cluster "prod" in "us-east-2" region with managed nodes
[ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial managed nodegroup
[ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-2 --cluster=prod'
[ℹ]  CloudWatch logging will not be enabled for cluster "prod" in "us-east-2"
[ℹ]  you can enable it with 'eksctl utils update-cluster-logging --region=us-east-2 --cluster=prod'
[ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "prod" in "us-east-2"
[ℹ]  2 sequential tasks: { create cluster control plane "prod", 2 sequential sub-tasks: { no tasks, create managed nodegroup "linux-nodes" } }
[ℹ]  building cluster stack "eksctl-prod-cluster"
[ℹ]  deploying stack "eksctl-prod-cluster"
[ℹ]  building managed nodegroup stack "eksctl-prod-nodegroup-linux-nodes"
[ℹ]  deploying stack "eksctl-prod-nodegroup-linux-nodes"
[ℹ]  waiting for the control plane availability...
[✔]  saved kubeconfig as "/home/sylvain/.kube/config"
[ℹ]  no tasks
[✔]  all EKS cluster resources for "prod" have been created
[ℹ]  nodegroup "linux-nodes" has 4 node(s)
[ℹ]  node "ip-192-168-2-127.us-east-2.compute.internal" is not ready
[ℹ]  node "ip-192-168-59-231.us-east-2.compute.internal" is ready
[ℹ]  node "ip-192-168-91-81.us-east-2.compute.internal" is ready
[ℹ]  node "ip-192-168-93-14.us-east-2.compute.internal" is ready
[ℹ]  waiting for at least 1 node(s) to become ready in "linux-nodes"
[ℹ]  nodegroup "linux-nodes" has 4 node(s)
[ℹ]  node "ip-192-168-2-127.us-east-2.compute.internal" is not ready
[ℹ]  node "ip-192-168-59-231.us-east-2.compute.internal" is ready
[ℹ]  node "ip-192-168-91-81.us-east-2.compute.internal" is ready
[ℹ]  node "ip-192-168-93-14.us-east-2.compute.internal" is ready
[ℹ]  kubectl command should work with "/home/sylvain/.kube/config", try 'kubectl get nodes'
[✔]  EKS cluster "prod" in "us-east-2" region is ready

````

eks cluster is here: 
https://us-east-2.console.aws.amazon.com/eks/home?region=us-east-2#/clusters

we can see here ir provision ec2 instance:
https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2#Instances:sort=instanceId

We can ssh on it (even via browser)


eks configured kubeconfig 
Thus we can do
https://github.com/scoulomb/soapui-docker/tree/master/kubernetes_integration_example#deliver-and-use-a-helm-package-in-helmhub

````shell script
sylvain@sylvain-hp:~$ helm repo add soapui https://helm-registry.github.io/soapui 
"soapui" has been added to your repositories
sylvain@sylvain-hp:~$ helm install test-helm-k8s-integration soapui/helm-k8s-integration  --set args.sender="robot.deploy@gmail.com" --set args.recipient="robot.deploy@gmail.com" --set args.password="<pwd>"
NAME: test-helm-k8s-integration
LAST DEPLOYED: Sun Aug  9 22:48:53 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
sylvain@sylvain-hp:~$ kubectl get po
No resources found in default namespace.
sylvain@sylvain-hp:~$ kubectl get cj
NAME             SCHEDULE    SUSPEND   ACTIVE   LAST SCHEDULE   AGE
nonreg-cronjob   * * * * *   False     1        10s             16s
sylvain@sylvain-hp:~$ kubectl get po -o wide
NAME                              READY   STATUS            RESTARTS   AGE   IP               NODE                                          NOMINATED NODE   READINESS GATES
nonreg-cronjob-1597006140-db65m   0/1     PodInitializing   0          18s   192.168.77.238   ip-192-168-93-14.us-east-2.compute.internal   <none>           <none>
sylvain@sylvain-hp:~$ kubectl get po -o wide
NAME                              READY   STATUS            RESTARTS   AGE   IP               NODE                                          NOMINATED NODE   READINESS GATES
nonreg-cronjob-1597006140-db65m   0/1     PodInitializing   0          29s   192.168.77.238   ip-192-168-93-14.us-east-2.compute.internal   <none>           <none>
sylvain@sylvain-hp:~$ kubectl get po -o wide
NAME                              READY   STATUS      RESTARTS   AGE   IP               NODE                                          NOMINATED NODE   READINESS GATES
nonreg-cronjob-1597006140-db65m   0/1     Completed   0          50s   192.168.77.238   ip-192-168-93-14.us-east-2.compute.internal   <none>           <none>
sylvain@sylvain-hp:~$ kubectl logs nonreg-cronjob-1597006140-db65m
robot.deploy@gmail.com
Credential provided
Content-Type: multipart/mixed; boundary="===============3221873905092894757=="
MIME-Version: 1.0
From: robot.deploy@gmail.com
To: robot.deploy@gmail.com
Date: Sun, 09 Aug 2020 20:49:43 +0000
Subject: non reg results

--===============3221873905092894757==
Content-Type: text/plain; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit

Find attached the report
--===============3221873905092894757==
Content-Type: application/octet-stream; Name="test_case_run_log_report.xml"
MIME-Version: 1.0
Content-Transfer-Encoding: base64
Content-Disposition: attachment; filename="test_case_run_log_report.xml"

PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPGNvbjp0ZXN0Q2FzZVJ1bkxv
ZyB0ZXN0Q2FzZT0iVGVzdENhc2UgMSIgdGltZVRha2VuPSIxMTIiIHN0YXR1cz0iRklOSVNIRUQi
IHRpbWVTdGFtcD0iMjAyMC0wOC0wOSAyMDo0OToyNCIgeG1sbnM6Y29uPSJodHRwOi8vZXZpd2Fy
ZS5jb20vc29hcHVpL2NvbmZpZyI+PGNvbjp0ZXN0Q2FzZVJ1bkxvZ1Rlc3RTdGVwIG5hbWU9IlJF
U1QgUmVxdWVzdCIgdGltZVRha2VuPSIxMTIiIHN0YXR1cz0iT0siIHRpbWVzdGFtcD0iMjAyMC0w
OC0wOSAyMDo0OToyNCIgZW5kcG9pbnQ9Imh0dHA6Ly93d3cuZ29vZ2xlLmZyLyIgaHR0cFN0YXR1
cz0iMjAwIiBjb250ZW50TGVuZ3RoPSIxMjczOCIgcmVhZFRpbWU9IjUiIHRvdGFsVGltZT0iMTMw
IiBkbnNUaW1lPSIyNCIgY29ubmVjdFRpbWU9IjQyIiB0aW1lVG9GaXJzdEJ5dGU9IjY0IiBodHRw
TWV0aG9kPSJHRVQiIGlwQWRkcmVzcz0iIi8+PC9jb246dGVzdENhc2VSdW5Mb2c+

--===============3221873905092894757==--

sent



sylvain@sylvain-hp:~$ kubectl get po -o wide
NAME                              READY   STATUS      RESTARTS   AGE     IP               NODE                                          NOMINATED NODE   READINESS GATES
nonreg-cronjob-1597006200-bbmbc   0/1     Completed   0          2m21s   192.168.94.222   ip-192-168-93-14.us-east-2.compute.internal   <none>           <none>
nonreg-cronjob-1597006260-hl28l   0/1     Completed   0          81s     192.168.77.238   ip-192-168-93-14.us-east-2.compute.internal   <none>           <none>
nonreg-cronjob-1597006320-c7np5   0/1     Completed   0          21s     192.168.76.30    ip-192-168-93-14.us-east-2.compute.internal   <none>           <none>
sylvain@sylvain-hp:~$ kubectl delete cj --all
cronjob.batch "nonreg-cronjob" deleted

````


Delete  (region not specified as fault one in AWS configure)

````
eksctl delete cluster prod
````


only worder nodes?

Move to serverless:
Cf [Serverless cases](1-microservice-on-aws-notes.mderverless-cases)
==> Fargate

### EKS based on Fargate

Here "Create your Amazon EKS cluster and compute", we choose Fargate

````shell script
eksctl create cluster \
--name prod-fargate \
--version 1.17 \
--region us-east-2 \
--fargate
````


Output is 

````
sylvain@sylvain-hp:~$ eksctl create cluster \
> --name prod-fargate \
> --version 1.17 \
> --region us-east-2 \
> --fargate
[ℹ]  eksctl version 0.25.0
[ℹ]  using region us-east-2
[ℹ]  setting availability zones to [us-east-2c us-east-2a us-east-2b]
[ℹ]  subnets for us-east-2c - public:192.168.0.0/19 private:192.168.96.0/19
[ℹ]  subnets for us-east-2a - public:192.168.32.0/19 private:192.168.128.0/19
[ℹ]  subnets for us-east-2b - public:192.168.64.0/19 private:192.168.160.0/19
[ℹ]  using Kubernetes version 1.17
[ℹ]  creating EKS cluster "prod-fargate" in "us-east-2" region with Fargate profile
[ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-2 --cluster=prod-fargate'
[ℹ]  CloudWatch logging will not be enabled for cluster "prod-fargate" in "us-east-2"
[ℹ]  you can enable it with 'eksctl utils update-cluster-logging --region=us-east-2 --cluster=prod-fargate'
[ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "prod-fargate" in "us-east-2"
[ℹ]  2 sequential tasks: { create cluster control plane "prod-fargate", create fargate profiles }
[ℹ]  building cluster stack "eksctl-prod-fargate-cluster"
[ℹ]  deploying stack "eksctl-prod-fargate-cluster"
[ℹ]  creating Fargate profile "fp-default" on EKS cluster "prod-fargate"
[ℹ]  created Fargate profile "fp-default" on EKS cluster "prod-fargate"
[ℹ]  "coredns" is now schedulable onto Fargate
[ℹ]  "coredns" is now scheduled onto Fargate
[ℹ]  "coredns" pods are now scheduled onto Fargate
[ℹ]  waiting for the control plane availability...
[✔]  saved kubeconfig as "/home/sylvain/.kube/config"
[ℹ]  no tasks
[✔]  all EKS cluster resources for "prod-fargate" have been created
[ℹ]  kubectl command should work with "/home/sylvain/.kube/config", try 'kubectl get nodes'
[✔]  EKS cluster "prod-fargate" in "us-east-2" region is ready
sylvain@sylvain-hp:~$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.100.0.1   <none>        443/TCP   19m
sylvain@sylvain-hp:~$ kubectl get nodes
NAME                                                    STATUS   ROLES    AGE   VERSION
fargate-ip-192-168-158-101.us-east-2.compute.internal   Ready    <none>   13m   v1.17.6-eks-4e7f64
fargate-ip-192-168-96-125.us-east-2.compute.internal    Ready    <none>   13m   v1.17.6-eks-4e7f64

````

Here:
https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2#Instances:sort=instanceId

We have no EC2 instance.

```
helm repo add soapui https://helm-registry.github.io/soapui 
helm install test-helm-k8s-integration soapui/helm-k8s-integration  --set args.sender="robot.deploy@gmail.com" --set args.recipient="robot.deploy@gmail.com" --set args.password="<pwd>"
```

Output is

````shell script
sylvain@sylvain-hp:~$ helm install test-helm-k8s-integration soapui/helm-k8s-integration  --set args.sender="robot.deploy@gmail.com" --set args.recipient="robot.deploy@gmail.com" --set args.password="<pwd>"

NAME: test-helm-k8s-integration
LAST DEPLOYED: Sun Aug  9 23:41:05 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
sylvain@sylvain-hp:~$ 
sylvain@sylvain-hp:~$ kubectl get cj
NAME             SCHEDULE    SUSPEND   ACTIVE   LAST SCHEDULE   AGE
nonreg-cronjob   * * * * *   False     0        <none>          52s

sylvain@sylvain-hp:~$ kubectl get po -o wide
NAME                              READY   STATUS            RESTARTS   AGE     IP                NODE                                                    NOMINATED NODE                                READINESS GATES
nonreg-cronjob-1597009320-mgrqz   0/1     Completed         0          4m17s   192.168.172.232   fargate-ip-192-168-172-232.us-east-2.compute.internal   <none>                                        <none>
nonreg-cronjob-1597009380-jtfbp   0/1     PodInitializing   0          3m17s   192.168.149.65    fargate-ip-192-168-149-65.us-east-2.compute.internal    <none>                                        <none>
nonreg-cronjob-1597009440-68tbf   0/1     Init:0/1          0          2m17s   192.168.97.86     fargate-ip-192-168-97-86.us-east-2.compute.internal     <none>                                        <none>
nonreg-cronjob-1597009500-h7fkv   0/1     Pending           0          77s     <none>            <none>                                                  5a03f38a38-0c83ef452c5d4d46847ece27dadee0a6   <none>
nonreg-cronjob-1597009560-rtqrf   0/1     Pending           0          17s     <none>            <none>                                                  f4c9951088-ccca009a5ee94ef98b00f88e2cf83d20   <none>
sylvain@sylvain-hp:~$ kubectl logs nonreg-cronjob-1597009320-mgrqz
robot.deploy@gmail.com
Credential provided
Content-Type: multipart/mixed; boundary="===============6044798383758861362=="
MIME-Version: 1.0
From: robot.deploy@gmail.com
To: robot.deploy@gmail.com
Date: Sun, 09 Aug 2020 21:46:02 +0000
Subject: non reg results

--===============6044798383758861362==
Content-Type: text/plain; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit

Find attached the report
--===============6044798383758861362==
Content-Type: application/octet-stream; Name="test_case_run_log_report.xml"
MIME-Version: 1.0
Content-Transfer-Encoding: base64
Content-Disposition: attachment; filename="test_case_run_log_report.xml"

PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPGNvbjp0ZXN0Q2FzZVJ1bkxv
ZyB0ZXN0Q2FzZT0iVGVzdENhc2UgMSIgdGltZVRha2VuPSIxMjQiIHN0YXR1cz0iRklOSVNIRUQi
IHRpbWVTdGFtcD0iMjAyMC0wOC0wOSAyMTo0NDoyMCIgeG1sbnM6Y29uPSJodHRwOi8vZXZpd2Fy
ZS5jb20vc29hcHVpL2NvbmZpZyI+PGNvbjp0ZXN0Q2FzZVJ1bkxvZ1Rlc3RTdGVwIG5hbWU9IlJF
U1QgUmVxdWVzdCIgdGltZVRha2VuPSIxMjQiIHN0YXR1cz0iT0siIHRpbWVzdGFtcD0iMjAyMC0w
OC0wOSAyMTo0NDoyMiIgZW5kcG9pbnQ9Imh0dHA6Ly93d3cuZ29vZ2xlLmZyLyIgaHR0cFN0YXR1
cz0iMjAwIiBjb250ZW50TGVuZ3RoPSIxMjc1OCIgcmVhZFRpbWU9IjE1IiB0b3RhbFRpbWU9IjEw
MTciIGRuc1RpbWU9IjI5IiBjb25uZWN0VGltZT0iNDkiIHRpbWVUb0ZpcnN0Qnl0ZT0iNTkiIGh0
dHBNZXRob2Q9IkdFVCIgaXBBZGRyZXNzPSIiLz48L2Nvbjp0ZXN0Q2FzZVJ1bkxvZz4=

--===============6044798383758861362==--

sent

````

And clean-up

````shell script
kubectl delete cj --all
sylvain@sylvain-hp:~$ eksctl delete cluster prod-fargate
[ℹ]  eksctl version 0.25.0
[ℹ]  using region us-east-2
[ℹ]  deleting EKS cluster "prod-fargate"
[ℹ]  deleting Fargate profile "fp-default"
[ℹ]  deleted Fargate profile "fp-default"
[ℹ]  deleted 1 Fargate profile(s)
[✔]  kubeconfig has been updated
[ℹ]  cleaning up AWS load balancers created by Kubernetes objects of Kind Service or Ingress
[ℹ]  1 task: { delete cluster control plane "prod-fargate" [async] }
[ℹ]  will delete stack "eksctl-prod-fargate-cluster"
[✔]  all cluster resources were deleted
````
<!--
STOP
-->