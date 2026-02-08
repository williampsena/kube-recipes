# Kind Hello World 🚀

A minimal Kubernetes setup using [Kind](https://kind.sigs.k8s.io/) with a simple whoami pod.

## 📋 Prerequisites

- Docker
- kubectl
- kind

## 🎯 What's Included

- **kind-cluster.yaml** - Multi-node cluster (1 control-plane + 1 worker)
- **whoami-pod.yaml** - Pod + NodePort Service
- **Makefile** - Automation commands

## 🚀 Quick Start

Create cluster:
```bash
make create
```

Deploy whoami:
```bash
kubectl apply -f whoami-pod.yaml
```

Access the app:
```bash
curl http://localhost:8080
```

## 🧹 Cleanup

```bash
make delete
```

## 📝 Notes

- The worker node is labeled with `app-purpose=apps`
- Pod uses nodeSelector to schedule on the worker
- NodePort 30080 is mapped to host port 8080

## 📚 Related Resources

- [🇧🇷 Suba um cluster Kubernetes com Kind em menos de 10 minutos](https://willsena.dev/suba-um-cluster-kubernetes-com-kind-em-menos-de-10-minutos/)