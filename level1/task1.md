The Nautilus DevOps team is initiating Kubernetes adoption for application migration. A team member has been tasked with creating a pod to test basic deployment capabilities.

Requirements:

Create a pod named pod-httpd using the httpd image with the latest tag (specify httpd:latest).
Set the label app: httpd_app.
Name the container httpd-container.
Note: The kubectl utility on the jump host is configured to work with the Kubernetes cluster. Values like pod names or namespaces may differ in your environment, but the core challenge remains the same.

Create Pod pod-httpd
Requirements
Pod name: pod-httpd
Image: httpd:latest
Label: app=httpd_app
Container name: httpd-container

Manifest
```
apiVersion: v1
kind: Pod
metadata:
  name: pod-httpd
  labels:
    app: httpd_app
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
```
Apply
```
kubectl apply -f pod-httpd.yaml
```
Verify
```
kubectl get pods
kubectl describe pod pod-httpd
```
