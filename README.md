# kubernetes-hello-world

Kubernetes studies annotations.

## Requirements

- fedora 35
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

