# Home Kubernetes Lab Setup

🚀 A comprehensive Kubernetes home lab running self-hosted applications including identity management, automation, and personal projects. Built with GitOps principles using Kustomize for Infrastructure as Code (IaC).

## 🏗️ Architecture Overview

This setup provides a complete home lab environment with:

- **Identity & Access Management**: Keycloak for SSO and user management
- **Workflow Automation**: n8n for business process automation
- **Personal Projects**: Resume website and tunnel application
- **Infrastructure Services**: SSL/TLS management, ingress, storage, and monitoring

## 📁 Project Structure

```
├── infrastructure/          # Core cluster infrastructure
│   ├── ssl/                # Certificate management (cert-manager, Let's Encrypt)
│   ├── ingress-nginx/      # Ingress controller configuration
│   ├── storage/            # Persistent storage (local-path-provisioner)
│   ├── portainer/          # Container management UI
│   ├── github-actions/     # CI/CD service accounts and RBAC
│   └── jobs/               # Maintenance jobs (image cleanup)
└── apps/                   # Application deployments
    ├── keycloak/          # Identity and access management
    ├── n8n/               # Workflow automation platform
    ├── resume/            # Personal resume website
    └── tunnel/            # Custom tunnel application
```

## 🚀 Applications

### Core Services

| Service       | URL                              | Description                    |
| ------------- | -------------------------------- | ------------------------------ |
| **Keycloak**  | `https://keycloak.kalkutin.dev`  | Identity and Access Management |
| **n8n**       | `https://n8n.kalkutin.dev`       | Workflow Automation Platform   |
| **Portainer** | `https://portainer.kalkutin.dev` | Container Management UI        |

### Personal Projects

| Service    | URL                           | Description                   |
| ---------- | ----------------------------- | ----------------------------- |
| **Resume** | `https://kalkutin.dev`        | Personal portfolio and resume |
| **Tunnel** | `https://tunnel.kalkutin.dev` | Amnezia based VPN solution    |

## 🛠️ Infrastructure Components

### SSL/TLS Management

- **cert-manager**: Automated certificate management
- **Let's Encrypt**: Production SSL certificates
- **Self-signed CA**: Development certificates

### Storage

- **local-path-provisioner**: Dynamic local storage provisioning

### Networking

- **NGINX Ingress**: HTTP/HTTPS traffic routing
- **NodePort**: Direct service access for specific services

### Security & Access

- **GitHub Actions RBAC**: Automated deployment permissions
- **Namespace isolation**: Separate dev/prod environments

## 🏠 Environment Setup

### Namespaces

- `infrastructure`: Core cluster services
- `apps-dev`: Development applications
- `apps-prod`: Production applications

### Node Configuration

- **Load Balancer Node**: `node-role.kubernetes.io/load-balancer`
- **Worker Nodes**:
  - `ru-worker-0.kalkutin.dev` (Russia region)
  - `md-worker-0.kalkutin.dev` (Moldova region)

## 🚀 Deployment

### Prerequisites

- Kubernetes cluster (1.33+)
- kubectl configured
- Kustomize (built into kubectl)

### Quick Start

1. **Deploy Infrastructure**

   ```bash
   kubectl apply -k infrastructure/
   ```

2. **Deploy Applications**

   ```bash
   kubectl apply -k apps/
   ```

3. **Verify Deployment**
   ```bash
   kubectl get pods -A
   kubectl get ingress -A
   ```

### Individual Component Deployment

```bash
# Deploy specific application
kubectl apply -k apps/keycloak/
kubectl apply -k apps/n8n/
kubectl apply -k apps/resume/

# Deploy infrastructure components
kubectl apply -k infrastructure/ssl/
kubectl apply -k infrastructure/portainer/
```

## 🔧 Configuration

### Environment-Specific Deployments

The setup uses Kustomize overlays for environment management:

```bash
# Production deployments
kubectl apply -k apps/keycloak/overlays/prod/
kubectl apply -k apps/n8n/overlays/prod/

# Development deployments
kubectl apply -k apps/tunnel/overlays/dev/
```

### SSL Certificate Management

Certificates are automatically managed via cert-manager with Let's Encrypt:

- **Production**: `letsencrypt-prod` ClusterIssuer
- **Staging**: `letsencrypt-staging` ClusterIssuer
- **Development**: `selfsigned-ca-issuer` ClusterIssuer

### Storage Configuration

All persistent data uses the `local-path` storage class:

```yaml
storageClassName: local-path
```

## 📊 Monitoring & Maintenance

### Metrics Server

Install metrics server for resource monitoring:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

For home lab setups, disable TLS verification:

```yaml
args:
  - --kubelet-insecure-tls
```

### Image Cleanup

Automated image cleanup runs via DaemonSet:

- Removes unused CRI-O images
- Runs on all nodes with appropriate tolerations

### Portainer Access

Access container management via Portainer UI with cluster-wide permissions.

## 🔐 Security

### RBAC Configuration

- GitHub Actions service account with deployment permissions
- Namespace-specific role bindings for apps-dev and apps-prod
- Cluster-wide access for infrastructure components

### Secret Management

- TLS certificates stored as Kubernetes secrets
- Application secrets (database credentials, API keys) managed per environment
- Separate secret management for dev/prod environments

## 🌐 DNS & Networking

### Domain Configuration

All services use the `kalkutin.dev` domain with appropriate subdomains:

- Main site: `kalkutin.dev`
- Services: `{service}.kalkutin.dev`

### Ingress Configuration

- SSL termination at ingress level
- HTTP to HTTPS redirects
- Custom caching headers for static content

## 📚 Technologies Used

- **Kubernetes**: Container orchestration
- **Kustomize**: Configuration management
- **cert-manager**: Certificate lifecycle management
- **NGINX Ingress**: HTTP/HTTPS routing
- **Keycloak**: Identity and access management
- **n8n**: Workflow automation
- **PostgreSQL**: Database for Keycloak
- **Docker**: Container runtime (CRI-O)

## 🤝 Contributing

This is a personal home lab setup, but feel free to:

- Open issues for questions or suggestions
- Submit PRs for improvements
- Use as reference for your own home lab

## 📝 License

This project is for educational and personal use. Individual components maintain their respective licenses.

---

**Author**: Evgenii Kalkutin  
**Website**: [kalkutin.dev](https://kalkutin.dev)  
**Environment**: Home Kubernetes Lab
