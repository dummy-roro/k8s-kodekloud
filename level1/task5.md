An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.18 with the latest updates.


Execute a rolling update for this application, integrating the nginx:1.18 image. The deployment is named nginx-deployment.

Ensure all pods are operational post-update.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.

Perform the rolling update by updating the deployment image:
```
kubectl set image deployment/nginx-deployment nginx=nginx:1.18
```
If the container name is not nginx, first check it:
```
kubectl describe deployment nginx-deployment
```
Then update using the correct container name:
```
kubectl set image deployment/nginx-deployment <container-name>=nginx:1.18
```
Monitor the rollout until it completes:
```
kubectl rollout status deployment/nginx-deployment
```
Verify that all pods are running the new image and are in the Running state:
```
kubectl get pods
kubectl describe deployment nginx-deployment
```
