
# GitLab Runner on Kubernetes

This directory contains the configuration and manifests to deploy a **GitLab Runner** in a Kubernetes cluster using native Kubernetes resources like **Deployments**, **ConfigMaps**, **Services**, and **RBAC**.

### Folder Structure

```bash
gitlab-runner/
├── configmap.yaml          # ConfigMap containing the GitLab Runner configuration (config.toml)
├── deployment.yaml         # Deployment manifest for GitLab Runner
├── service.yaml            # Service for exposing Prometheus metrics
├── rbac.yaml               # Role and RoleBinding for GitLab Runner permissions
└── README.md
```       

### Documentation for this setup
Prerequisites
A Kubernetes cluster with kubectl configured.
A valid GitLab instance and registration token.
Permissions to create Kubernetes resources in the gitlab-runner namespace.
Deployment Steps

### 1. Create the Namespace
If the gitlab-runner namespace doesn’t exist, create it:

```sh
kubectl create namespace gitlab-runner
```

### 2. Apply the RBAC Configuration

The rbac.yaml file grants the GitLab Runner the necessary permissions to manage jobs and interact with Kubernetes resources:
```sh
kubectl apply -f rbac.yaml -n gitlab-runner
```

### 3. Apply the ConfigMap
The configmap.yaml contains the config.toml configuration for the GitLab Runner:
```sh
kubectl apply -f configmap.yaml -n gitlab-runner
```
### 4. Deploy the GitLab Runner
Apply the deployment.yaml to create the GitLab Runner pod:
```sh
kubectl apply -f deployment.yaml -n gitlab-runner
```
### 5. Apply the Service
The service.yaml exposes the Prometheus metrics on port 9252 for monitoring:
```sh
kubectl apply -f service.yaml -n gitlab-runner
```
Monitoring the GitLab Runner
Check the Pod Status
```sh
kubectl get pods -n gitlab-runner
```

View Logs for Troubleshooting

```sh
kubectl logs <pod-name> -n gitlab-runner
```

### Port Forward for Metrics Access
To access Prometheus metrics locally:
```sh
kubectl port-forward svc/gitlab-runner-metrics 9252:9252 -n gitlab-runner
```

Then visit: http://localhost:9252/metrics

### Cleanup
To remove all resources related to the GitLab Runner:
```sh
kubectl delete -f rbac.yaml -n gitlab-runner
kubectl delete -f configmap.yaml -n gitlab-runner
kubectl delete -f deployment.yaml -n gitlab-runner
kubectl delete -f service.yaml -n gitlab-runner
```
### Future Improvements
Liveness and Readiness Probes: Add health checks in the deployment.yaml to monitor the runner's health.
GitOps Integration: Use GitOps tools like ArgoCD or Flux for automated deployments.
CI/CD Pipeline: Setup GitLab CI/CD to validate and deploy Kubernetes manifests.

Maintainer: dheva
Date: February 2025
