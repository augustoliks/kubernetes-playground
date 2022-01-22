# kubernetes-hello-world

Kubernetes studies annotations.

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
