# OpsLedger Deployment Guide

Use this sequence to validate and explain a complete OpsLedger deployment.

## 1. Project Goal

OpsLedger demonstrates a production-style delivery path for a Java web application on AWS EKS. The project covers infrastructure, CI/CD, image publishing, Kubernetes deployment, and supporting runtime services.

## 2. Repository Layout

- `opsledger-infra`: Terraform VPC and EKS.
- `opsledger-app-deploy`: Java app, Dockerfile, Helm chart, Kubernetes manifests, and CI/CD.

## 3. Infrastructure Workflow

Show:

```bash
terraform init
terraform validate
terraform plan
terraform apply
```

Explain the VPC, private worker nodes, public load balancer subnets, and EKS managed node groups.

## 4. CI/CD Workflow

Show the GitHub Actions workflow:

- Maven test.
- Checkstyle report.
- SonarCloud quality analysis.
- Docker image build.
- ECR publish.
- Helm deployment to EKS.

The workflow is manual-only by default. Run it first with `publish=false` for quality checks, then rerun with `publish=true` after repository secrets, variables, and the Kubernetes `opsledger-secret` are ready.

## 5. Application Validation

Open the OpsLedger dashboard and point out:

- Ledger summary cards.
- Account records.
- Approval queue.
- Cache status.
- Service health panel.

## 6. Kubernetes Validation

Use:

```bash
kubectl get pods
kubectl get svc
kubectl get ingress
helm list
```

Explain how each workload maps to the application architecture.

## Screenshot Checklist

Capture final screenshots after deployment:

- OpsLedger dashboard.
- GitHub Actions successful run.
- SonarCloud project analysis.
- ECR image repository.
- EKS workloads.
- Helm release.
- Ingress/application URL.
