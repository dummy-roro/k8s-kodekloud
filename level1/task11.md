A junior DevOps team member encountered difficulties deploying a stack on the Kubernetes cluster. The pod fails to start, presenting errors. Let's troubleshoot and rectify the issue promptly.


There is a pod named webserver, and the container within it is named httpd-container, its utilizing the httpd:latest image.

Additionally, there's a sidecar container named sidecar-container using the ubuntu:latest image.

Identify and address the issue to ensure the pod is in the running state and the application is accessible.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


For this type of troubleshooting task, first inspect the pod to identify why it is failing:
```
k get all
k describe pod webserver
```
Don't forget to check the events

Edit the pod and save changes (change the image, it should be nginx:latest)
```
kubectl edit pod webserver
```
To check
```
k get all 
k describe pod
```
