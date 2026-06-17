Useful Kubernetes Commands Cheat Sheet

i use "k" instead of typing full "kubectl"

Nodes
```
k get nodes
```
Pods
```
k get po
k describe po <pod-name>
k delete po <pod-name>
```
Deployments
```
k get deployments
k describe deployment <deployment-name>
k rollout status deployment <deployment-name>
k rollout undo deployment <deployment-name>
k rollout history deployment <deployment-name>
```
ReplicaSets
```
k get rs
k describe rs <replicaset-name>
```
Namespaces
```
k get ns
k create ns <namespace-name>
```
CronJobs
```
k get cronjobs
k describe cronjob <cronjob-name>
```
Resource Inspection
```
k top pods
k describe pod <pod-name>
```
YAML Validation
```
k apply --dry-run=client -f <filename>.yaml
k explain pod
k explain deployment
```
This covers all tasks: Pods, Namespaces, Deployments, Rolling Updates, Rollbacks, Resource Limits, ReplicaSets, and CronJobs.
