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
$ kubectl create -f k8Deploy/VotingAppUsingPods
$ kubectl create -f k8Deploy/VotingAppServices

```
2. Run the following command to create the Deployments and Services objects:
```
$ kubectl create -f k8Deploy/VotingAppUsingDeployment
$ kubectl create -f k8Deploy/VotingAppServices

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

**Some common commands**

https://kubernetes.io/docs/reference/kubectl/#resource-types
 
*kubectl [command] [TYPE] [NAME] [flags] <br />
&nbsp; &nbsp; commands: eg. create, get, describe, delete <br />
&nbsp; &nbsp; type: resource type,eg. pod, service, deployment, replicaSets <br />
&nbsp; &nbsp; name: Specifies the name of the resource <br />*

kubectl get pods <br />
kubectl get pods -o wide <br />
kubectl describe pods <name> <br />
kubectl create -f <file.yaml> <br />
kubectl apply -f <file.yaml> <br />
kubectl delete pod <name> <br />
 

kubectl edit replicaset <name> <br />
kubectl replace -f <replica_config.yaml> <br />
kubectl scale replicaset <replicaSet_name> --replica={number}  <br />
kubectl explain replicaset | grep VERSION <br /> 
 
kubectl rollout status <deployment_name> <br />
kubectl rollout history <deployment_nname> <br />
kubectl rollout undo <deployment_name> <br />
kubectl create -f deployment.yaml --record  <br />



