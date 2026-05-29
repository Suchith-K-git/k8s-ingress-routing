# Kubernetes Ingress Setup — Django Notes App + NGINX

## Project Overview

This project demonstrates Kubernetes Ingress with path-based routing using:

* Django Notes Application
* NGINX Application
* NGINX Ingress Controller

The Ingress routes traffic:

* `/` → Django Notes App
* `/nginx` → NGINX Application

---

# Project Structure

```text id="eq4zqr"
k8s-path-based-routing/
│
├── namespace.yml
├── django-deployment.yml
├── django-service.yml
├── nginx-deployment.yml
├── nginx-service.yml
├── ingress.yml
└── README.md
```

---

# Architecture

```text id="9tdu8n"
Internet
    ↓
NGINX Ingress Controller
    ↓
Ingress Resource
   ↙        ↘
/            /nginx
↓              ↓
Django App     NGINX App
```

---

# Step 1 — Create Namespace

Apply namespace:

```bash id="sxjlwm"
kubectl apply -f namespace.yml
```

Verify:

```bash id="l1nl8h"
kubectl get ns
```

---

# Step 2 — Deploy Django Application

Apply deployment:

```bash id="n0x22w"
kubectl apply -f django-deployment.yml
```

Apply service:

```bash id="d6mhul"
kubectl apply -f django-service.yml
```

Verify:

```bash id="u4vyto"
kubectl get pods -n notes-app
kubectl get svc -n notes-app
```

---

# Step 3 — Deploy NGINX Application

Apply deployment:

```bash id="qccq8o"
kubectl apply -f nginx-deployment.yml
```

Apply service:

```bash id="6qgj0j"
kubectl apply -f nginx-service.yml
```

Verify:

```bash id="dg7bnn"
kubectl get pods -n notes-app
kubectl get svc -n notes-app
```

---

# Step 4 — Install NGINX Ingress Controller

Install the NGINX Ingress Controller:

```bash id="b5pr5u"
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

Verify controller pods:

```bash id="x29rse"
kubectl get pods -n ingress-nginx
```

Verify services:

```bash id="x6yl3h"
kubectl get svc -n ingress-nginx
```

---

# Step 5 — Apply Ingress Resource

Apply ingress configuration:

```bash id="5zt6p8"
kubectl apply -f ingress.yml
```

Verify ingress:

```bash id="8p6zk4"
kubectl get ingress -n notes-app
```

---

# Step 6 — Port Forwarding (Optional)

If external IP is not available, use port forwarding.

## Port Forward Ingress Controller

```bash id="jlwmfq"
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:80 --address 0.0.0.0
```

Now access:

## Django Notes App

```text id="0o6x1u"
http://localhost:8080/
```

## NGINX Application

```text id="06snw3"
http://localhost:8080/nginx
```

---

# Verify Resources

## Check Pods

```bash id="5zzkps"
kubectl get pods -A
```

## Check Services

```bash id="f4w65h"
kubectl get svc -A
```

## Check Ingress

```bash id="4vqlm3"
kubectl get ingress -A
```

---

# Useful Commands

## Describe Ingress

```bash id="l5gmm0"
kubectl describe ingress nginx-notes-ingress -n notes-app
```

## Check Logs

```bash id="v5ik1h"
kubectl logs <pod-name> -n notes-app
```

## Delete All Resources

```bash id="4i7tm0"
kubectl delete -f .
```

---

# Access Applications

## Using External IP

### Django Notes App

```text id="uqv45l"
http://<EXTERNAL-IP>/
```

### NGINX Application

```text id="6v0m6t"
http://<EXTERNAL-IP>/nginx
```

---

# Learning Outcomes

By completing this project, you will understand:

* Kubernetes Namespace
* Deployments
* ClusterIP Services
* NGINX Ingress Controller
* Ingress Resource
* Path-Based Routing
* Port Forwarding
* Traffic Routing in Kubernetes

---

# Author

Suchith K
