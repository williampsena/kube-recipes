# Kube Recipes

A collection of Kubernetes examples and patterns using modern tools.

## 📁 Projects

### [Gateway API Demo](./apps/gateway-api/README.md)
Learn about Kubernetes Gateway API using Istio as the gateway provider with observability through Prometheus.

**What you'll find:**
- Istio service mesh integration with minimal profile
- Gateway API and HTTPRoute configuration
- Sample "whoami" application deployment
- Prometheus monitoring setup
- Production-ready patterns for modern routing

**Quick start:**
```bash
cd apps/gateway-api
make create      # Create KinD cluster
make install     # Install Gateway API CRDs, Istio & Prometheus
make deploy      # Deploy demo application
make port-forward # Access the services
```

**Learn more:** [Gateway API Documentation](./apps/gateway-api/README.md)

---

### [Kind Hello World](./apps/kind-hello-world/README.md)
A minimal Kubernetes setup example using Kind with a simple whoami pod - perfect for beginners.

**What you'll find:**
- Multi-node Kind cluster (1 control-plane + 1 worker)
- Pod and NodePort Service examples
- Simple automation with Makefile
- Perfect starting point for Kubernetes learning

**Quick start:**
```bash
cd apps/kind-hello-world
make create      # Create KinD cluster
kubectl apply -f whoami-pod.yaml  # Deploy whoami
curl http://localhost:8080        # Access the app
```

---

### [Kratix Platform](./apps/kratix/README.md)
Learn how to build reusable application patterns using the Kratix platform framework.

**What you'll find:**
- Two Promise examples: Whoami and Nginx
- Local Kind cluster setup with Kratix and Cert-Manager
- Health checks, resource limits, and rolling updates
- Complete Makefile with simple commands

**Quick start:**
```bash
cd apps/kratix
make up      # Create cluster
make whoami  # Deploy Whoami (http://localhost:8080)
make nginx   # Deploy Nginx (http://localhost:31080)
make status  # Check everything
make down    # Clean up
```

**Learn more:** [Kratix Examples](./apps/kratix/PROMISE_EXAMPLES.md)

---

### [Traefik Examples](./apps/traefik/README.md)
Traefik reverse proxy setup examples for different Kubernetes environments.

**Included:**
- **k3s** - Traefik with local k3s cluster
- **minikube** - Traefik with minikube cluster

Each includes:
- Ingress configuration
- Middleware examples
- Authentication setup

---

## 🚀 Prerequisites

- **Docker** - For Kind cluster
- **Kind** - Kubernetes in Docker
- **kubectl** - Kubernetes CLI
- **make** - Build automation

## 📚 How to Use

1. Pick a project in `./apps`
2. Read the project's documentation
3. Follow the quick start commands
4. Customize for your needs

## 🎯 Examples Overview

| Project | Type | Best For | Level |
|---------|------|----------|-------|
| Kind Hello World | Minimal Setup | Getting started with Kubernetes | Beginner |
| Gateway API Demo | Service Mesh | Modern routing with Istio & Prometheus | Intermediate |
| Kratix | Framework | Learning Promise patterns & reusable apps | Advanced |
| Traefik k3s | Config | Local reverse proxy with k3s | Intermediate |
| Traefik minikube | Config | Local reverse proxy with minikube | Intermediate |

---

**Each project is self-contained with its own Makefile and documentation.**
