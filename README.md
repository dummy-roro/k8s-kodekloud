Useful Kubernetes Commands Cheat Sheet
Pods
```
kubectl get pods
kubectl describe pod <pod-name>
kubectl delete pod <pod-name>
```
Deployments
```
kubectl get deployments
kubectl describe deployment <deployment-name>
kubectl rollout status deployment <deployment-name>
kubectl rollout undo deployment <deployment-name>
kubectl rollout history deployment <deployment-name>
```
ReplicaSets
```
kubectl get rs
kubectl describe rs <replicaset-name>
```
Namespaces
```
kubectl get ns
kubectl create ns <namespace-name>
```
CronJobs
```kubectl get cronjobs
kubectl describe cronjob <cronjob-name>
```
Resource Inspection
```
kubectl top pods
kubectl describe pod <pod-name>
```
YAML Validation
```
kubectl apply --dry-run=client -f file.yaml
kubectl explain pod
kubectl explain deployment
```
This covers all tasks: Pods, Namespaces, Deployments, Rolling Updates, Rollbacks, Resource Limits, ReplicaSets, and CronJobs.
