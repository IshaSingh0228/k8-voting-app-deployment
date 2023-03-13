Example Voting App
=========

A simple distributed application running across multiple Docker containers.

Getting started
---------------
Install below tools:
1. kubectl  : https://kubernetes.io/docs/tasks/tools/#minikube
2. container or virtual machine manager such as Docker or VirtualBox
3. minikube:  https://minikube.sigs.k8s.io/docs/start/

Run the app in Kubernetes
-------------------------

The folder k8s-specifications contains the yaml specifications of the Voting App's services in 2-ways:
1. Using Pods and Services
2. Uisng Deployments and Services

1. Run the following command to create the Pods and Services objects:
```
$ kubectl create -f k8s-specifications/
deployment "db" created
service "db" created
deployment "redis" created
service "redis" created
deployment "result" created
service "result" created
deployment "vote" created
service "vote" created
deployment "worker" created
```
2. Run the following command to create the Deployments and Services objects:
```
$ kubectl create -f k8s-specifications/
deployment "db" created
service "db" created
deployment "redis" created
service "redis" created
deployment "result" created
service "result" created
deployment "vote" created
service "vote" created
deployment "worker" created
```
The vote interface is then available on port 31000 on each host of the cluster, the result one is available on port 31001.

or minikube service <service_name> --url

Architecture
-----

![Architecture diagram](architecture.png)

* A front-end web app in [Python](/vote) or [ASP.NET Core](/vote/dotnet) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) or [NATS](https://hub.docker.com/_/nats/) queue which collects new votes
* A [.NET Core](/worker/src/Worker), [Java](/worker/src/main) or [.NET Core 2.1](/worker/dotnet) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) or [TiDB](https://hub.docker.com/r/dockersamples/tidb/tags/) database backed by a Docker volume
* A [Node.js](/result) or [ASP.NET Core SignalR](/result/dotnet) webapp which shows the results of the voting in real time


Note
----

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.

Some common commands 
----
https://kubernetes.io/docs/reference/kubectl/#resource-types
 
kubectl [command] [TYPE] [NAME] [flags] 
  *commands: eg. create, get, describe, delete
  *type: resource type,eg. pod, service, deployment, replicaSets
  *name: Specifies the name of the resource
  
*kubectl get pods
*kubectl get pods -o wide
*kubectl describe pods <name>
*kubectl create -f <file.yaml>
*kubectl apply -f <file.yaml>
*kubectl delete pod <name>

*kubectl edit replicaset <name>
*kubectl replace -f <replica_config.yaml>
*kubectl scale replicaset <replicaSet_name> --replica=<number>
*kubectl explain replicaset | grep VERSION

*kubectl rollout status <deployment_name>
*kubectl rollout history <deployment_nname>
*kubectl rollout undo <deployment_name>
*kubectl create -f deployment.yaml --record 




