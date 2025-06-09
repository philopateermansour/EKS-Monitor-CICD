# EKS Monitor

EKS Monitor is an infrastructure-as-code project that provisions a complete AWS EKS environment for deploying, monitoring, and managing a guestbook application. The stack leverages Terraform for AWS resource provisioning, Helm for Kubernetes package management, and Jenkins with Kaniko for CI/CD automation.

---

## Features

- **Network Automation:** VPC, public/private subnets, NAT gateway, route tables, and internet gateway.
- **Security:** Security groups, IAM roles, and SSH key management for secure cluster and bastion access.
- **Compute:** Bastion host for secure access, EKS cluster, and managed node groups.
- **Container Registry:** ECR repositories for frontend and backend Docker images.
- **Identity Management:** Fine-grained IAM roles and policies for EKS, nodes, and CI/CD.
- **Monitoring & CI/CD:** Automated deployment of Prometheus, Grafana, and Jenkins via Helm.
- **Kubernetes Resources:** Manifests for guestbook app, MySQL, Redis, and monitoring stack.
- **Pipeline:** Jenkinsfile for Kaniko-based Docker image builds and deployment to ECR.

---

## Directory Structure

```
eks-monitor/
├── Terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── terraform.tfvars
│   ├── backend.tf
│   ├── outputs.tf
│   └── Modules/
│       ├── Network/
│       ├── Security/
│       ├── Compute/
│       ├── ECR/
│       ├── EKS/
│       ├── Identity/
│       ├── Helm/
│       └── kubernetes/
├── Jenkinsfile
├── guestbook/        
├── .gitignore
└── README.md
```

---

## Getting Started

### Prerequisites

- [Terraform](https://www.terraform.io/downloads.html) >= 1.0
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker](https://docs.docker.com/get-docker/)
- [Helm](https://helm.sh/docs/intro/install/)
- AWS account with sufficient permissions

### Setup

1. **Clone the repository:**
    ```sh
    git clone https://github.com/yourusername/eks-monitor.git
    cd eks-monitor/Terraform
    ```

2. **Configure variables:**
    - Edit `terraform.tfvars` to match your AWS environment and preferences.

3. **Initialize and apply Terraform:**
    ```sh
    terraform init
    terraform apply
    ```

4. **Configure kubectl:**
    ```sh
    aws eks update-kubeconfig --region <region> --name <cluster_name>
    ```

5. **Deploy the guestbook app (if not automated):**
    ```sh
    helm install guestbook ../guestbook
    ```

6. **Set up Jenkins and CI/CD:**
    - Jenkins is deployed via Helm and configured to use Kaniko for building and pushing Docker images to ECR.
    - The provided `Jenkinsfile` defines the pipeline.

---

## CI/CD Pipeline

- **Jenkinsfile:**  
  Defines a Kubernetes-native pipeline using Kaniko to build and push Docker images for frontend and backend services to ECR, triggered on code changes.

---

## Security

- Sensitive variables (like passwords) should be stored in `secrets.tfvars` (excluded from git).
- IAM roles and policies are provisioned with least-privilege principles.

---

## Monitoring

- **Prometheus** and **Grafana** are deployed via Helm for cluster and application monitoring.
- Grafana admin credentials are managed via Terraform variables.
