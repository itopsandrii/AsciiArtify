# Proof of Concept: ArgoCD Deployment on Local Kubernetes (k3d)

## Overview

This document describes the steps to deploy **ArgoCD** on a local Kubernetes cluster using **k3d**.  
The goal is to set up **ArgoCD** in a **dedicated namespace**, expose the **web interface**, and access it locally via **http://localhost:8080**.

Inspired by the official [ArgoCD Getting Started Guide](https://argo-cd.readthedocs.io/en/stable/getting_started/).

---

## Prerequisites

- A running **k3d** cluster  
- **kubectl** configured to access your cluster  
- **Internet access** to pull ArgoCD manifests

---

## Step-by-step Deployment Instructions

### 1. Create a Dedicated Namespace

```bash
kubectl create namespace argocd
```

---

### 2. Set the Namespace as Default

```bash
kubectl config set-context --current --namespace=argocd
```

---

### 3. Install ArgoCD into the `argocd` Namespace

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

### 4. Verify Deployment Status

```bash
kubectl get pods
```

Wait until all pods have **STATUS: Running**.

---

### 5. Access the ArgoCD Web Interface

Expose the ArgoCD server using port forwarding:

```bash
kubectl port-forward svc/argocd-server 8080:443
```

Now open your browser and navigate to:

[http://localhost:8080](http://localhost:8080)

---

### 6. Login Credentials

- **Username:** `admin`
- **Password:** The initial admin password is stored in the following Kubernetes secret:

```bash
kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
```

Use these credentials to log in.

---

## References

- [ArgoCD Official Getting Started Guide](https://argo-cd.readthedocs.io/en/stable/getting_started/)
- [ArgoCD GitHub Repository](https://github.com/argoproj/argo-cd)

---

## Repository

Please visit the repository to review this documentation:

[https://github.com/<username>/AsciiArtify](https://github.com/<username>/AsciiArtify)