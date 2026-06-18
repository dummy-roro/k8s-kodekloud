The Nautilus DevOps team needs a time check pod created in a specific Kubernetes namespace for logging purposes. Initially, it's for testing, but it may be integrated into an existing cluster later. Here's what's required:


Create a pod called time-check in the datacenter namespace. The pod should contain a container named time-check, utilizing the busybox image with the latest tag (specify as busybox:latest).

Create a config map named time-config with the data TIME_FREQ=10 in the same namespace.

Configure the time-check container to execute the command: while true; do date; sleep $TIME_FREQ;done. Ensure the result is written /opt/sysops/time/time-check.log. Also, add an environmental variable TIME_FREQ in the container, fetching its value from the config map TIME_FREQ key.

Create a volume log-volume and mount it at /opt/sysops/time within the container.

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


Create the Namespace, ConfigMap and Pod in the datacenter namespace.
```
vi time-check.yaml
```
```
apiVersion: v1
kind: Namespace
metadata:
  name: datacenter
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: datacenter
data:
  TIME_FREQ: "10"
---
apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: datacenter
spec:
  containers:
  - name: time-check
    image: busybox:latest
    env:
    - name: TIME_FREQ
      valueFrom:
        configMapKeyRef:
          name: time-config
          key: TIME_FREQ
    command:
    - /bin/sh
    - -c
    - while true; do date >> /opt/sysops/time/time-check.log; sleep $TIME_FREQ; done
    volumeMounts:
    - name: log-volume
      mountPath: /opt/sysops/time
  volumes:
  - name: log-volume
    emptyDir: {}
```
To apply these
```
kubectl apply -f time-check.yaml
```
Check
```
kubectl get pod time-check -n datacenter
kubectl get configmap time-config -n datacenter
kubectl exec -n datacenter time-check -- cat /opt/sysops/time/time-check.log
```
