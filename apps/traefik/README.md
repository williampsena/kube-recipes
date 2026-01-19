# Traefik Examples

Traefik reverse proxy setup examples for different Kubernetes environments.

## 📋 Available Examples

### [k3s Setup](./k3s/README.md)
Complete Traefik configuration for a local k3s cluster.

**Includes:**
- Traefik deployment
- Whoami application with ingress
- Local domain configuration
- Middleware examples

**Quick start:**
```bash
cd k3s
# Follow setup instructions in README.md
```

---

### [Minikube Setup](./minikube/README.md)
Traefik setup optimized for Minikube development.

**Includes:**
- Traefik deployment
- Dashboard ingress
- Whoami application
- Minikube tunnel configuration

**Quick start:**
```bash
cd minikube
minikube start
minikube addons enable ingress
# Follow setup instructions in README.md
```

---

## 🎯 Comparison

| Feature | k3s | Minikube |
|---------|-----|----------|
| **Setup** | Lightweight | Full VM |
| **Performance** | Fast | Moderate |
| **Local DNS** | Manual hosts | Minikube DNS |
| **Best For** | Linux systems | Cross-platform testing |

---

## 📚 Each subdirectory contains:
- Detailed README with setup steps
- YAML files for deployment
- Configuration examples
- Testing instructions

**Choose the one that matches your Kubernetes distribution and pick a subdirectory to get started.**
