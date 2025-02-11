# Intro

This repository contains practical examples for working with Kubernetes.

# Minikube

## How to start

```shell
sudo systemctl start docker
minikube stop
minikube delete
minikube start --driver=docker
minikube addons enable ingress
minikube addons enable ingress-dns
```