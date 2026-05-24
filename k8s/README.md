# Kubernetes Deployment Guide — Hello World App

## Prerequisites

* Docker installed and running
* Minikube installed
* kubectl installed

---

## Start Kubernetes Cluster

```bash
minikube start --driver=docker
```

Verify:

```bash
kubectl get nodes
```

---

## Deploy Application

Apply all Kubernetes manifests:

```bash
kubectl apply -f k8s/
```

---

## Verify Deployment

```bash
kubectl get pods
kubectl get deployments
kubectl get services
```

---

## Access the Application

```bash
minikube service hello-world-service
```

---

## Verify Autoscaling (HPA)

```bash
kubectl get hpa
```

---

## Check Logs

```bash
kubectl logs <pod-name>
```

---

## Stop CronJobs (optional)

```bash
kubectl patch cronjob hello-world-cron -p '{"spec" : {"suspend" : true }}'
```

