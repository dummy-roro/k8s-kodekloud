We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.



Create a pod named volume-share-devops.


For the first container, use image debian with latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-devops-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/official.


For the second container, use image debian with the latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-devops-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/cluster.


Volume name should be volume-share of type emptyDir.


After creating the pod, exec into the first container i.e volume-container-devops-1, and just for testing create a file official.txt with the content Welcome to xFusionCorp Industries under the mounted path of first container i.e /tmp/official.


The file official.txt should be present under the mounted path /tmp/cluster on the second container volume-container-devops-2 as well, since they are using a shared volume.


Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


Use the following commands on the jump host.

Create the Pod
```
vi /tmp/volume-share-devops.yaml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  containers:
  - name: volume-container-devops-1
    image: debian:latest
    command: ["/bin/sh", "-c", "sleep 3600"]
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/official

  - name: volume-container-devops-2
    image: debian:latest
    command: ["/bin/sh", "-c", "sleep 3600"]
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/cluster

  volumes:
  - name: volume-share
    emptyDir: {}
```

Apply it:
```
kubectl apply -f /tmp/volume-share-devops.yaml
```
Wait for the pod to become Running:
```
kubectl get pod volume-share-devops
```
Create the test file in the first container
```
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- \
sh -c 'echo "Welcome to xFusionCorp Industries" > /tmp/official/official.txt'
```
Verify from the second container
```
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- \
cat /tmp/cluster/official.txt
```
Expected output:
```
Welcome to xFusionCorp Industries
```
You can also verify the file exists:
```
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- \
ls -l /tmp/cluster
```
