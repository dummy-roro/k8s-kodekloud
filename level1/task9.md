The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below:


Create a job named countdown-devops.

The spec template should be named countdown-devops (under metadata), and the container should be named container-countdown-devops

Utilize image debian with latest tag (ensure to specify as debian:latest), and set the restart policy to Never.

Execute the command sleep 5

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.

Create the Job using the following manifest:
```
vi countdown-devops.yaml
```
```
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-devops
spec:
  template:
    metadata:
      name: countdown-devops
    spec:
      restartPolicy: Never
      containers:
      - name: container-countdown-devops
        image: debian:latest
        command:
        - sleep
        - "5"
```
Apply it:
```
kubectl apply -f countdown-devops.yaml
```
Verify the job:
```
kubectl get jobs
kubectl describe job countdown-devops
kubectl get pods
```
Check Job Logs
```
kubectl logs countdown-devops-xxx
```
Purpose: Verify the command executed (no output expected for sleep 5, but confirms successful execution).
