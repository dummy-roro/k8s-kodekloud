An application deployed on the Kubernetes cluster requires an update with new features developed by the Nautilus application development team. The existing setup includes a deployment named nginx-deployment and a service named nginx-service. Below are the necessary changes to be implemented without deleting the deployment and service:


1.) Modify the service nodeport from 30008 to 32165

2.) Change the replicas count from 1 to 5

3.) Update the image from nginx:1.19 to nginx:latest

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.

You can perform all three changes without deleting any resources.
```
k get all -o wide
```
1. Scale the deployment to 5 replicas
```
kubectl scale deployment nginx-deployment --replicas=5
```
2. Update the image to nginx:latest

First check the container name if needed:
```
kubectl get deployment nginx-deployment -o jsonpath='{.spec.template.spec.containers[*].name}'
```
Then update the image (assuming the container name is nginx):
```
kubectl set image deployment/nginx-deployment nginx=nginx:latest
```
If the container name differs, use that name instead.

3. Change the NodePort from 30008 to 32165

Edit the service:
```
kubectl edit svc nginx-service
```
Change:
```
nodePort: 30008
```
to:
```
nodePort: 32165
```
Verify
```
kubectl get deployment nginx-deployment
kubectl get pods
kubectl get svc nginx-service
kubectl rollout status deployment/nginx-deployment
curl http://<ip>:<port>
```
