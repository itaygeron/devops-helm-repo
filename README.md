# Hello World App Helm Chart

This repository contains a Helm chart for deploying the `hello-world-app` Kubernetes application.

The chart is published as an OCI Helm chart in Docker Hub.

## Prerequisites

Make sure you have the following installed:

- Kubernetes cluster, for example Minikube
- `kubectl`
- `helm`
- Docker Hub access, if the chart repository is private

Check your tools:

```bash
kubectl version --client
helm version
```

Check that your cluster is running:

```bash
kubectl get nodes
```

If you are using Minikube:

```bash
minikube start
```

## Login to Docker Hub Registry

If the chart is private, login first:

```bash
helm registry login registry-1.docker.io
```

Use your Docker Hub username and password/token when prompted.

## Install the Application

Install the Helm chart from Docker Hub:

```bash
helm install hello-world-app oci://registry-1.docker.io/itaygeron/hello-world-app --version 0.1.0
```

If your Docker Hub namespace is `itaygeron2`, use this instead:

```bash
helm install hello-world-app oci://registry-1.docker.io/itaygeron2/hello-world-app --version 0.1.0
```

## Verify the Installation

Check the Helm release:

```bash
helm list
```

Check the Kubernetes resources:

```bash
kubectl get all
kubectl get configmap
kubectl get secret
kubectl get hpa
kubectl get cronjob
```

Check the pods:

```bash
kubectl get pods
```

View pod logs:

```bash
kubectl logs -l app=hello-world-app
```

## Access the Application

If the Service uses `NodePort` and you are running on Minikube, run:

```bash
minikube service hello-world-service
```

Or get the service URL:

```bash
minikube service hello-world-service --url
```

Then open the printed URL in your browser.

You can also check the service manually:

```bash
kubectl get svc hello-world-service
```

## Install with Custom Values

You can override chart values during installation.

Example:

```bash
helm install hello-world-app oci://registry-1.docker.io/itaygeron/hello-world-app \
  --version 0.1.0 \
  --set replicaCount=2 \
  --set image.tag=v1.0
```

Or use a custom values file:

```bash
helm install hello-world-app oci://registry-1.docker.io/itaygeron/hello-world-app \
  --version 0.1.0 \
  -f values.yaml
```

## Upgrade the Application

After publishing a new chart version, upgrade the release:

```bash
helm upgrade hello-world-app oci://registry-1.docker.io/itaygeron/hello-world-app --version 0.1.1
```

To upgrade with custom values:

```bash
helm upgrade hello-world-app oci://registry-1.docker.io/itaygeron/hello-world-app \
  --version 0.1.1 \
  -f values.yaml
```

## Uninstall the Application

Remove the app from the cluster:

```bash
helm uninstall hello-world-app
```

Verify that the resources were removed:

```bash
kubectl get all
kubectl get secret
kubectl get configmap
```

## Useful Helm Commands

Render the chart locally without installing:

```bash
helm template hello-world-app oci://registry-1.docker.io/itaygeron/hello-world-app --version 0.1.0
```

Show chart information:

```bash
helm show chart oci://registry-1.docker.io/itaygeron/hello-world-app --version 0.1.0
```

Show default values:

```bash
helm show values oci://registry-1.docker.io/itaygeron/hello-world-app --version 0.1.0
```

## Troubleshooting

### Resource already exists

If you previously applied the YAML files manually, Helm may fail because the resources already exist.

Delete the old manually-created resources first:

```bash
kubectl delete -f ./your-yaml-folder/
```

Then install again:

```bash
helm install hello-world-app oci://registry-1.docker.io/itaygeron/hello-world-app --version 0.1.0
```

### Check release status

```bash
helm status hello-world-app
```

### Debug rendered YAML

```bash
helm template hello-world-app oci://registry-1.docker.io/itaygeron/hello-world-app --version 0.1.0 --debug
```

