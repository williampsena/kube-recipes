# Introduction

This repository provides an example of how to run Traefik locally using k3s. Follow the instructions to set up and configure Traefik in your local k3s environment.

# Requirements
## k3s

```shell
curl -sfL https://get.k3s.io | sh -
```

Starting k3s

```shell
sudo systemctl start k3s
```

Ensure that `kubectl` is configured to interact with your k3s cluster by setting the `KUBECONFIG` environment variable:

```shell
sudo cp /etc/rancher/k3s/k3s.yaml $HOME/.kube/config-k3s
sudo chown $USER:$USER $HOME/.kube/config-k3s

# add to your profile $HOME/.profile
export KUBECONFIG=$HOME/.kube/config-k3s
```

# Setup

```shell
kubectl apply -f whoami-deployment.yaml \
              -f whoami-middleware.yaml \
              -f whoami-ingress.yaml
```

# Configure local domain

Adds to /etc/hosts

```shell
echo "127.0.0.1 whoami.local-k3s traefik.local-k3s" | sudo tee -a /etc/hosts
```


# Testing

```shell
curl http://whoami.local-k3s/
# Hostname: whoami

curl http://traefik.local-k3s/
# Moved Permanently

curl "http://localhost"
# 404 Not found
```

# Cleanup

```shell
kubectl delete -f traefik-dashboard.yaml

kubectl delete -f whoami-deployment.yaml \
               -f whoami-middleware.yaml \
               -f whoami-ingress.yaml
```

# Traefik dashboard

## SOLUTION 1: Path Traefik Services

```shell
kubectl patch service traefik -n kube-system --patch "$(cat traefik-service-patch.yaml)"
```

## Protected Routes over HTTP and HTTPS

To secure your routes, configure Traefik to use both HTTP (`web`) and HTTPS (`websecure`) entry points. Additionally, use HTTP basic authentication middleware for extra protection. However, it is recommended to restrict access to these routes via a VPN for enhanced security.


```shell
kubectl apply -f traefik-auth-middleware.yaml \
              -f traefik-secret.yaml \
              -f traefik-dashboard-ingress.yaml
```

## SOLUTION 2: Using forward

```shell
kubectl port-forward -n kube-system deployment/traefik 9000:9000
```

## SOLUTION 3: Editing service to expose port 9000 (manually)
```shell
kubectl edit services traefik -n kube-system
```

```yaml
spec:
    ports:
    - name: traefik
      port: 9000
      targetPort: 9000
```