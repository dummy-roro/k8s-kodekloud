The Nautilus DevOps team is gearing up to deploy applications on a Kubernetes cluster for migration purposes. A team member has been tasked with creating a ReplicaSet outlined below:



Create a ReplicaSet using nginx image with latest tag (ensure to specify as nginx:latest) and name it nginx-replicaset.


Apply labels: app as nginx_app, type as front-end.


Name the container nginx-container. Ensure the replica count is 4.


Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.

Create the ReplicaSet with the following manifest:
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    app: nginx_app
    type: front-end
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx_app
      type: front-end
  template:
    metadata:
      labels:
        app: nginx_app
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
```
Save it as nginx-replicaset.yaml and apply it:
```
kubectl apply -f nginx-replicaset.yaml
```
Verify the ReplicaSet and pods:
```
kubectl get rs
kubectl get pods --show-labels
```
