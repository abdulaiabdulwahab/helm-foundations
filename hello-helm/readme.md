# Helm Foundations Project

## Overview

This project is a beginner-friendly introduction to **Helm**, the package manager for Kubernetes.

The goal is to learn how Helm charts work, how values are passed into Kubernetes templates, and how Helm manages application releases.

The project deploys a simple **NGINX application** to a local Kubernetes cluster using Helm.

---

## What I Learned

This project covers:

- Creating a Helm chart
- Understanding `Chart.yaml`
- Using `values.yaml`
- Creating Kubernetes templates
- Using Helm variables
- Validating Helm charts
- Rendering Kubernetes manifests
- Installing applications with Helm
- Upgrading Helm releases
- Viewing release history
- Rolling back deployments
- Uninstalling Helm releases
- Troubleshooting Helm and Kubernetes deployments

---

## Technologies Used

- Helm
- Kubernetes
- Docker Desktop Kubernetes
- kubectl
- NGINX

---

## Project Structure

```text
helm-foundations/
│
└── hello-helm/
    ├── Chart.yaml
    ├── values.yaml
    │
    └── templates/
        ├── deployment.yaml
        └── service.yaml
```

---

## Prerequisites

Verify that Helm is installed:

```bash
helm version
```

Verify kubectl:

```bash
kubectl version --client
```

Verify the Kubernetes cluster:

```bash
kubectl get nodes
```

For this project, Docker Desktop Kubernetes can be used as the local Kubernetes cluster.

---

## Create the Helm Chart

```bash
helm create hello-helm
```

The default templates were removed so the Deployment and Service could be created manually.

---

## Validate the Chart

Run:

```bash
helm lint ./hello-helm
```

This checks the Helm chart for common configuration and syntax problems.

---

## Render the Kubernetes Manifests

```bash
helm template hello ./hello-helm
```

For additional debugging information:

```bash
helm template hello ./hello-helm --debug
```

This shows the Kubernetes YAML that Helm generates from the templates.

---

## Test the Installation

Run a dry run before deploying:

```bash
helm install hello ./hello-helm \
  --namespace helm-lab \
  --create-namespace \
  --dry-run \
  --debug
```

A dry run allows the chart to be tested without creating resources in Kubernetes.

---

## Install the Helm Release

```bash
helm install hello ./hello-helm \
  --namespace helm-lab \
  --create-namespace
```

Verify the Helm release:

```bash
helm list -n helm-lab
```

Verify Kubernetes resources:

```bash
kubectl get all -n helm-lab
```

---

## Upgrade the Release

After changing a value such as:

```yaml
replicaCount: 3
```

upgrade the application:

```bash
helm upgrade hello ./hello-helm \
  --namespace helm-lab
```

Verify:

```bash
kubectl get pods -n helm-lab
```

---

## View Release History

```bash
helm history hello \
  --namespace helm-lab
```

Helm stores revisions every time a release is upgraded.

---

## Roll Back a Deployment

To return to an earlier release:

```bash
helm rollback hello 1 \
  --namespace helm-lab
```

Check the release:

```bash
helm status hello \
  --namespace helm-lab
```

---

## Troubleshooting

### Check the Helm chart

```bash
helm lint ./hello-helm
```

### Render templates

```bash
helm template hello ./hello-helm --debug
```

### Check Helm releases

```bash
helm list -A
```

### Check Pods

```bash
kubectl get pods -n helm-lab
```

### Inspect a failing Pod

```bash
kubectl describe pod <POD_NAME> \
  -n helm-lab
```

### View container logs

```bash
kubectl logs <POD_NAME> \
  -n helm-lab
```

A useful troubleshooting workflow is:

```text
helm lint
    ↓
helm template --debug
    ↓
helm install --dry-run --debug
    ↓
helm status
    ↓
kubectl describe
    ↓
kubectl logs
```

---

## Clean Up

Remove the Helm release:

```bash
helm uninstall hello \
  --namespace helm-lab
```

Delete the namespace if it is no longer needed:

```bash
kubectl delete namespace helm-lab
```

---

## Key Helm Commands

```bash
helm create
helm lint
helm template
helm install
helm list
helm status
helm upgrade
helm history
helm rollback
helm uninstall
```

---

## Outcome

By completing this project, I gained hands-on experience with the core concepts of Helm and learned how Helm simplifies Kubernetes application deployment, configuration, upgrades, release history, and rollback management.