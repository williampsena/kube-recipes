# Kratix + Whoami Setup
```mermaid
graph TD
    A["Kind Cluster"] --> B["Kratix Controller<br/>(Promise Platform)"]
    A --> C["Whoami App<br/>2 Replicas"]
    B -.->|manages via Promise| C
    C --> D["Service<br/>NodePort 30080"]
    D --> E["HostPort 8080"]
    E --> F["http://localhost:8080"]
    style A fill:#b3e5fc,stroke:#01579b,stroke-width:2px,color:#000
    style B fill:#ffe0b2,stroke:#e65100,stroke-width:2px,color:#000
    style C fill:#f3e5f5,stroke:#4a148c,stroke-width:2px,color:#000
    style D fill:#c8e6c9,stroke:#1b5e20,stroke-width:2px,color:#000
    style E fill:#c8e6c9,stroke:#1b5e20,stroke-width:2px,color:#000
    style F fill:#c8e6c9,stroke:#1b5e20,stroke-width:2px,color:#000
```

## Quick Start

```bash
# Create cluster and deploy everything
make up

# Check status
make status

# Remove cluster
make down
```

## Structure

### Essential Files
- **Makefile** - Simplified build targets
- **kind-config.yaml** - Kind cluster configuration with port mappings
- **iac/whoami-deployment.yaml** - Whoami application (fully consolidated)

### Kept Files
- **iac/whoami-deployment.yaml** - The ONLY YAML needed, fully production-ready with:
  - Namespace
  - Deployment (2 replicas with health checks)
  - Service (NodePort + HostPort)
  - Resource limits
  - Rolling update strategy

## Commands

```bash
make up                 # Create cluster and install everything
make down               # Delete cluster
make whoami            # Deploy whoami app
make whoami-status     # Check whoami status
make whoami-logs       # Follow whoami logs
make whoami-delete     # Remove whoami deployment
make status            # Show full cluster status
make logs              # Show Kratix controller logs
make help              # Show all available commands
```

## Access Points

Once deployed:

- **HostPort (Direct)**: `http://localhost:8080`
- **NodePort (Service)**: `http://localhost:30080`
- **Internal (cluster DNS)**: `http://whoami.whoami-demo.svc.cluster.local`

Both work without kubectl port-forward due to Kind port mappings in `kind-config.yaml`.

## Notes

- Kratix Promise support is documented as non-functional due to upstream bug
- Manual Kubernetes deployment is stable and reliable
- All targets are documented with `make help`
- Internal targets (starting with `_`) are helpers not meant for direct use
