# DAPR geeting started

## Prerequsite

Before do: 
- Kubernetes basic: https://github.com/scoulomb/myk8s
- Not essential but some Azure 900 background
- APIM: https://docs.microsoft.com/en-us/learn/paths/architect-api-integration/ 
- Pluralsigth APIM course: https://app.pluralsight.com/library/courses/microsoft-azure-api-management-strategy-designing/table-of-contents
- Publish subscribe (Kafka class on ps): https://app.pluralsight.com/library/courses/apache-kafka-getting-started/table-of-contents


## Hello kube 

https://github.com/dapr/quickstarts

It shows service to service invocation + state management and k8s deployment


To get Redis password do

```
export REDIS_PASSWORD=$(sudo kubectl get secret --namespace default redis -o jsonpath="{.data.redis-password}" | base64 --decode)
```
To connect to your Redis server, run a Redis pod that you can use as a client:

```
 sudo  kubectl run --namespace default redis-client --restart='Never'  --env REDIS_PASSWORD=$REDIS_PASSWORD  --image docker.io/bitnami/redis:6.2.6-debian-10-r0 --command -- sleep infinity
```

Use the following command to attach to the pod:

```
sudo kubectl exec --tty -i redis-client \
   --namespace default -- bash
```
and Connect using the Redis CLI:

```   
redis-cli -h redis-master -a J4ABucm68o
redis-cli -h redis-master -a $REDIS_PASSWORD
```
To connect to your database from outside the cluster execute the following commands:

```
kubectl port-forward --namespace default svc/my-release-redis-master 6379:6379 &
redis-cli -h 127.0.0.1 -p 6379 -a $REDIS_PASSWORD
```	


https://stackoverflow.com/questions/8078018/get-redis-keys-and-values-at-command-prompt


Then we can see entry in Redis

```
	redis-master:6379> KEYS *
1) "nodeapp||order"
redis-master:6379> type nodeapp||order
hash
redis-master:6379> hgetall nodeapp||order
1) "data"
2) "{\"orderId\":2386}"
3) "version"
4) "2392"
redis-master:6379>
```


Side note: 

`kubectl port-forward service/nodeapp 8080:80` in readme alt is to use node port


## Publish suscribe 

https://github.com/dapr/quickstarts

Read this: https://docs.dapr.io/developing-applications/building-blocks/pubsub/howto-publish-subscribe/

In the example we do a programmtic subscription and not via the Custom resource.

We can use as pubsub component Kafka or Redis STREAM.
https://docs.dapr.io/reference/components-reference/supported-pubsub/


Had some issue with redis in crashloop
https://github.com/bitnami/charts/issues/5181
Solved by 

```
sudo minikube delete
sudo rm -rf /tmp/juju*
sudo minikube start --vm-driver=none
sudo dapr init -k # as done in hello kube.
```

LAB requires redis setup `sudo helm install redis bitnami/redis`

I used not port and not port forwarding because 
https://stackoverflow.com/questions/33517646/unable-to-do-port-forwarding-socat-not-found-kubernetes-on-docker

To view message published do 

````
sudo kubectl logs react-form-656879b44d-9m6pn -c react-form
````
Some error

I realized I had issue because I was not in folder with message body.

Then I was able to message in publish server + redis
But it does not reach the suscriber (osef...)
Because I had initialized dapr init after pod creation, so dapr pod was not injected !!

```
sylvain@sylvain-hp:~$ sudo kubectl get po
NAME                                 READY   STATUS    RESTARTS   AGE
node-subscriber-5b94cf775b-vm587     1/1     Running   0          48m
python-subscriber-645d8b8d4d-vhbt9   1/1     Running   0          48m
react-form-656879b44d-9m6pn          2/2     Running   0          16m
redis-client                         1/1     Running   0          30m
redis-client2                        1/1     Running   0          10m
redis-master-0                       1/1     Running   0          51m
redis-replicas-0                     1/1     Running   0          51m
redis-replicas-1                     1/1     Running   0          50m
redis-replicas-2                     1/1     Running   0          50m
sylvain@sylvain-hp:~$ sudo kubectl delete po node-subscriber-5b94cf775b-vm587 python-subscriber-645d8b8d4d-vhbt9
pod "node-subscriber-5b94cf775b-vm587" deleted
pod "python-subscriber-645d8b8d4d-vhbt9" deleted
sylvain@sylvain-hp:~$ sudo kubectl get po
NAME                                 READY   STATUS    RESTARTS   AGE
node-subscriber-5b94cf775b-sfs66     2/2     Running   0          76s
python-subscriber-645d8b8d4d-wcshr   2/2     Running   0          76s
```

understand concept => OK fully completed STOP

## Bindings with Kafka

https://github.com/dapr/quickstarts

What is the differnce between binding and pub/sub?
See https://docs.dapr.io/developing-applications/building-blocks/bindings/bindings-overview/

in particular 

> Remove the complexities of connecting to, and polling from, messaging systems such as queues and message buses


Binding component can be Kafa, or Redis
https://docs.dapr.io/reference/components-reference/supported-bindings/



How to check topics inside Kafala

```shell
sudo kubectl -n kafka exec -it dapr-kafka-0  /bin/sh
/opt/bitnami/kafka/bin

kafka-topics.sh --list  --zookeeper 10.101.245.134
kafka-topics.sh --describe sample  --zookeeper 10.101.245.134 (k8s service)

kafka-console-consumer.sh  --topic sample --from-beginning  --bootstrap-server 
```

## Note with helm 

https://stackoverflow.com/questions/57961162/helm-install-unknown-flag-name

````
scoulombel@NCEL96011 MINGW64 ~/dev/quickstarts (master)
$ git diff --staged
diff --git a/hello-kubernetes/README.md b/hello-kubernetes/README.md
index 5766d1c1bf75..2bc5e8bd834a 100644
--- a/hello-kubernetes/README.md
+++ b/hello-kubernetes/README.md
@@ -72,7 +72,7 @@ Dapr can use a number of different state stores (Redis, CosmosDB, DynamoDB, Cass

 1. Follow [these steps](https://docs.dapr.io/getting-started/configure-redis/) to create a Redis store.
 2. Once your store is created, add the keys to the `redis.yaml` file in the `deploy` directory.
-   > **Note:** the `redis.yaml` file provided in this quickstart will work securely out-of-the-box with a Redis installed with `helm install bitnami/redis`. If you have your own Redis setup, replace the `redisHost` value with your own Redis master address, and the redisPassword with your own Secret. You can learn more [here](https://docs.dapr.io/operations/components/component-secrets/).
+   > **Note:** the `redis.yaml` file provided in this quickstart will work securely out-of-the-box with a Redis installed with `helm install redis bitnami/redis`. If you have your own Redis setup, replace the `redisHost` value with your own Redis master address, and the redisPassword with your own Secret. You can learn more [here](https://docs.dapr.io/operations/components/component-secrets/).
 3. Apply the `redis.yaml` file and observe that your state store was successfully configured!

 <!-- STEP
diff --git a/pub-sub/deploy/redis.yaml b/pub-sub/deploy/redis.yaml
index 7fdc61f6cbeb..bc810afad892 100644
--- a/pub-sub/deploy/redis.yaml
+++ b/pub-sub/deploy/redis.yaml
@@ -6,7 +6,7 @@ spec:
   type: pubsub.redis
   version: v1
   metadata:
-  # These settings will work out of the box if you use `helm install
+  # These settings will work out of the box if you use `helm install redis
   # bitnami/redis`.  If you have your own setup, replace
   # `redis-master:6379` with your own Redis master address, and the
````

## Next Azure integration 

#### We follow this tuto
https://github.com/dapr/samples/tree/master/dapr-apim-integration

#### Setup azure CLI
https://docs.microsoft.com/fr-fr/cli/azure/install-azure-cli-linux?pivots=apt


#### Specific command for my account


````
export AZ_SUBSCRIPTION_ID="858de4c9-b430-4db3-950f-50ed9f451a1b"
export AZ_RESOURCE_GROUP="scoulomb1"

az apim create --name $APIM_SERVICE_NAME \
               --subscription $AZ_SUBSCRIPTION_ID \
               --resource-group $AZ_RESOURCE_GROUP \
               --publisher-email "cloudz@coulombel.net" \
               --publisher-name "scoulomb"


````

#### Zipkin was need 

I had error 

````
error: a container name must be specified for pod echo-service-987cd84fb-sxwz5, choose one of: [service daprd]
sylvain@sylvain-hp:~/samples/dapr-apim-integration$ sudo kubectl logs echo-service-987cd84fb-sxwz5 -c daprd
{"app_id":"echo-service","instance":"echo-service-987cd84fb-sxwz5","level":"info","msg":"starting Dapr Runtime -- version 1.4.3 -- commit a8ee30180e1183e2a2e4d00c283448af6d73d0d0","scope":"dapr.runtime","time":"2021-10-22T18:02:19.367935073Z","type":"log","ver":"1.4.3"}
{"app_id":"echo-service","instance":"echo-service-987cd84fb-sxwz5","level":"info","msg":"log level set to: debug","scope":"dapr.runtime","time":"2021-10-22T18:02:19.368001631Z","type":"log","ver":"1.4.3"}
{"app_id":"echo-service","instance":"echo-service-987cd84fb-sxwz5","level":"info","msg":"metrics server started on :9090/","scope":"dapr.metrics","time":"2021-10-22T18:02:19.368367232Z","type":"log","ver":"1.4.3"}
{"app_id":"echo-service","instance":"echo-service-987cd84fb-sxwz5","level":"debug","msg":"Config error: rpc error: code = Unknown desc = error getting configuration: Configuration.dapr.io \"tracing\" not found","scope":"dapr.runtime","time":"2021-10-22T18:02:19.377306437Z","type":"log","ver":"1.4.3"}
{"app_id":"echo-service","instance":"echo-service-987cd84fb-sxwz5","level":"fatal","msg":"error loading configuration: rpc error: code = Unknown desc = error getting configuration: Configuration.dapr.io \"tracing\" not found","scope":"dapr.runtime","time":"2021-10-22T18:02:19.377367567Z","type":"log","ver":"1.4.3"}

````


Solved by 

https://docs.dapr.io/operations/monitoring/tracing/supported-tracing-backends/zipkin/


#### Anotation was missing in fiven example


````
Warning  FailedCreate  71s (x16 over 3m55s)  replicaset-controller  Error creating: admission webhook "sidecar-injector.dapr.io" denied the request: value for the dapr.io/app-id annotation is empty
````


We added app-id in event-subscriber

````
 cat k8s/event-subscriber.yaml
      annotations:
        dapr.io/app-id: "tutu"
        dapr.io/enabled: "true"
        dapr.io/id: "event-subscriber"
        dapr.io/port: "8080"
````

#### Use cluster ip or node

If no load balancer we can use cluster ip (or nodeport)

````
export GATEWAY_IP=$(kubectl get svc demo-apim-gateway -o jsonpath='{.spec.clusterIP'})
````

#### Be careful to secret (space)

#### Gatway not recoginozed from Azure 

It was still not working so we used directly generated yaml from the console
(deployment and infrastructure > gateway > deployment section) and it worked

Hower we need to add dappr annotation!!!

Here is the generated file augmented with dapr annotation


````
sylvain@sylvain-hp:~/samples/dapr-apim-integration$ cat demo-apim-gateway.yaml
# NOTE: Before deploying to a production environment, please review the documentation -> https://aka.ms/self-hosted-gateway-production
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: demo-apim-gateway-env
data:
  config.service.endpoint: "https://dapr-apim-demo.management.azure-api.net/subscriptions/858de4c9-b430-4db3-950f-50ed9f451a1b/resourceGroups/scoulomb1/providers/Microsoft.ApiManagement/service/dapr-apim-demo?api-version=2021-01-01-preview"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-apim-gateway
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo-apim-gateway
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0
      maxSurge: 25%
  template:
    metadata:
      labels:
        app: demo-apim-gateway
      annotations:
        dapr.io/enabled: "true"
        dapr.io/app-id: "demo-apim-gateway"
        dapr.io/config: "tracing"
        dapr.io/log-as-json: "true"
        dapr.io/log-level: "debug"
    spec:
      terminationGracePeriodSeconds: 60
      containers:
      - name: demo-apim-gateway
        image: mcr.microsoft.com/azure-api-management/gateway:latest
        ports:
        - name: http
          containerPort: 8080
        - name: https
          containerPort: 8081
        readinessProbe:
          httpGet:
            path: /internal-status-0123456789abcdef
            port: http
            scheme: HTTP
          initialDelaySeconds: 0
          periodSeconds: 5
          failureThreshold: 3
          successThreshold: 1
        env:
        - name: config.service.auth
          valueFrom:
            secretKeyRef:
              name: demo-apim-gateway-token
              key: value
        envFrom:
        - configMapRef:
            name: demo-apim-gateway-env
---
apiVersion: v1
kind: Service
metadata:
  name: demo-apim-gateway
spec:
  type: LoadBalancer
  externalTrafficPolicy: Local
  ports:
  - name: http
    port: 80
    targetPort: 8080
  - name: https
    port: 443
    targetPort: 8081
  selector:
    app: demo-apim-gateway


````

Difference is the images only (latest vs beta).
Running original repo image still worked so error came from config/copy paste I guess.

We can also get the db content from redis

I managed to have binding to db + service invocation working
but not pub sub osef in Azure APIM demo

## Other notes

- PR for helmchart has to give the name in helm when creating components in dpar demo 
- PR in DAPR APIM demo they we should not have a HTTP 200 but it is actually a 204
- we could link to dapr standalone demo without APIM but sufficient we got the principle or only in one of them

For instance pub/sub via UI policy on get api

````
    <inbound>
        <publish-to-dapr pubsub-name="pubsub" topic="C" content-type="application/json" response-variable-name="resp-var-name">@(context.Request.Body.As<string>())</publish-to-dapr>
        <base />
    </inbound>
````

And then 

````
curl -i -X POST -d '{"message": "Message on ABCDEFggg"}'
    -H "Content-Type: application/json"      -H "Ocp-Apim-Subscription-Key: ${AZ_API_SUB_KEY}"
-H "Ocp-Apim-Trace: true"      "http://${GATEWAY_IP}/echo"
kubectl logs --selector app=python-subscriber -c python-subscriber
````

we have weirdly a 404 but it worked, bug?

````
127.0.0.1 - - [22/Oct/2021 22:13:55] "POST /A HTTP/1.1" 500 -
C: {'specversion': '1.0', 'source': 'demo-apim-gateway', 'type': 'com.dapr.event.sent', 'topic': 'C', 'id': 'be7d40fa-5fa8-4eec-9923-0b921d4dcf25', 'datacontenttype': 'application/json', 'pubsubname': 'pubsub', 'traceid': '00-d17e894824cdcad72ec5a161fbd34a32-a41c5c06405f2ea7-01', 'data': {'message': 'Message on ABCDEFggg'}}
Received message "Message on ABCDEFggg" on topic "C"
````

Also note that DAPR sends message in format expected by API server 
(convert via kubectl edit dapr api svc to nodeport)
We could do from https://docs.dapr.io/reference/api/pubsub_api/

````
curl -X POST "http://localhost:30323/v1.0/publish/pubsub/C" \
-H "Content-Type: application/json" \
-d '{"message": "Message on DIRECT CURL" }'
````

Here we also had a 500 (weirdly...), this approach replaces the js server. 

ST CLEAR OK - SUFFICIENT FOR UNDERSTANDING
COME BACK IF DEMO AND PR
YES STOP OK SUFFIT

question: does dapr use back end k8s service, or speak directly to sidecar??