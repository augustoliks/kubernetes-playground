# kubernetes-hello-world

Annontations of the Kubernetes studies.

My workstation it's composed by:

- Fedora 35
- kubectl
- nerdctl
- containerd
- containernetworking-plugins
- kvm2

## Commands

Create cluster with Minikube.

```bash
$ minikube start --container-runtime=containerd --driver=kvm2 --nodes=3
```

Autocomplete 

```bash
echo 'source <(kubectl completion bash)' >> ~/.bashrc
```

List contexts

```bash
kubectl kubecconfig get-contexts
```

Change context

```bash
kubectx CONTEXT
```

Cluster status

```bash
kubectl get componentstatuses
```

Get nodes:

```bash
kubectl get nodes

NAME           STATUS   ROLES                  AGE    VERSION
minikube       Ready    control-plane,master   2m7s   v1.22.3
minikube-m02   Ready    <none>                 73s    v1.22.3
minikube-m03   Ready    <none>                 29s    v1.22.3
```

Get more information

```bash
kubectl get nodes -o json -o wide
```

Get value with JSONPath sytax

```bash
kubectl get nodes minikube -o jsonpath --template={.spec.podCIDR} 
```

Resource hot edit 

```bash
kubectl edit <resource> <object-name>
```

Resources label

```bash
kubectl label pods bar color=red
kubectl label pods bar color=blue --overwrite
kubectl label pods bar color-
```

list containers in POD

```bash
kubectl get pods <pod-name> -o jsonpath='{.spec.containers[*].name}'
```

view container log in POD

```bash
kubectl logs <pod-name> -c <container-name> -f
```

Exec command in containers

```bash
kubectl exec -it <pod-name> -- bash
```

follow main process in POD (like logs, but with stdin available)

```bash
kubectl attach -it <pod-name>
```

Copy files to and from pod

```bash
kubectl <pod-name>:/var/local/xtpo /home/user/
```

Expose POD ports

```bash
kubectl port-forward <pod-name> 8080:80 
```

Expose SERVICE ports

```bash
kubectl port-forward services/<service-name> 
```

Get ReplicaSet details

```bash
kubectl describe rs kuard
```

Check POD owner references

```bash
kubectl get pods kuard-xccrl -o yaml | grep -A 10 ownerReferences
```

Scale Replicas (imperative way)

```bash
kubectl scale replicaset <replicaset-name> --replicas=4 
```

Enable Autoscale (HPA), 2..5 replicas by 80% cpu usage

```bash
kubectl autoscale rs kuard --min=2 --max=5 --cpu-percent=80
```

Get deployment labels

```bash
kubectl get deployments kuard -o jsonpath --template {.spec.selector.matchLabels}
```

Filter Replica Set by label

```bash
kubectl get replicasets -l run=kuard
```

Backup deployment

```bash
kubectl get deployment <deployment-name> -o yaml > bkp-deploy.yaml
kubectl replace -f <deployment-file .yml> --save-config
```


Get rollout history

```bash
$ kubectl rollout history deployment kuard 


deployment.apps/kuard 
REVISION  CHANGE-CAUSE
1         <none>
2         update to green kuard

```

> __note__: _'update to green kuard'_ message it's define in deployment manifest (.yml) annontation

```yaml
spec:

...

  template:
    metadata:
      annotations:
        kubernetes.io/change-cause: "update to green kuard"
```

Get rollout informations of the specific version

```bash
$ kubectl rollout history deployment kuard --revision=2


deployment.apps/kuard with revision #2
Pod Template:
  Labels:	pod-template-hash=5789685c8d
	run=kuard
  Annotations:	kubernetes.io/change-cause: update to green kuard
  Containers:
   kuard:
    Image:	gcr.io/kuar-demo/kuard-amd64:green
    Port:	<none>
    Host Port:	<none>
    Environment:	<none>
    Mounts:	<none>
  Volumes:	<none>
```

Undo rollout

```bash
kubectl rollout undo deployments kuard
```

Rollout to specific version

```bash
kubectl rollout undo deployment kuard --to-revision=3
```

