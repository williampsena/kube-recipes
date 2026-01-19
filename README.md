# Kube Recipes

A collection of Kubernetes examples and patterns using modern tools.

## 📁 Projects

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

| Project | Type | Best For |
|---------|------|----------|
| Kratix | Framework | Learning Promise patterns |
| Traefik k3s | Config | Local reverse proxy setup |
| Traefik minikube | Config | Minikube testing |

---

**Each project is self-contained with its own Makefile and documentation.**
