# kubernetes-hello-world

Simple example created for explore kubernetes features. 

## Requirements

Requirements    | Version
---             |---
kind            | 0.11.0
kubectl         | v1.21.1

## Commands

```shell
kind create cluster --name=cluster-go-server
```

```shell
kubectl cluster-info --context kind-cluster-go-server
```

```shell
cd application/
docker build -t go-ws . 
```

```shell
kind load docker-image go-ws --name=cluster-go-server 
```

```shell
cd k8s/
kubectl apply -f deployment.yml 
kubectl get deploy
```

```shell
kubectl apply -f service.yml
kubectl get services
```

```shell
kubectl get pod
kubectl get replicaset
kubectl get rs
kubectl rollout history deployment.v1.apps/go-ws-deployment
ubectl rollout history deployment.v1.apps/go-ws-deployment --revision=2
```

```shell
kubectl rollout undo deployment.v1.apps/go-ws-deployment --to-revision=3 
kubectl port-forward service/go-ws-service 8080:8090
```

