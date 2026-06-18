The Nautilus DevOps team has already deployed a ReplicaSet to host an application that requires a highly available infrastructure. Your task is to expose the application running in the existing ReplicaSet by creating a Kubernetes NodePort Service.

Follow the specifications below to create the Service and ensure the application pods are accessible:


A ReplicaSet named nginx-replicaset is already running in the cluster.

The pods managed by the ReplicaSet use the following labels:
Assign labels app as nginx_app, and type as front-end.

Create a NodePort Service named nginx-service to expose the application.

Set the NodePort to 30080.

Expose port 80 of the application.

Note: Do not delete or modify the configuration of the deployed ReplicaSet application.

```
k get rs
```
```
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       44s
thor@jump-host ~$ k describe rs nginx-replicaset 
Name:         nginx-replicaset
Namespace:    default
Selector:     app=nginx_app,type=front-end
Labels:       <none>
Annotations:  <none>
Replicas:     3 current / 3 desired
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx_app
           type=front-end
  Containers:
   nginx-container:
    Image:         nginx:latest
    Port:          80/TCP
    Host Port:     0/TCP
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  61s   replicaset-controller  Created pod: nginx-replicaset-wshc5
  Normal  SuccessfulCreate  61s   replicaset-controller  Created pod: nginx-replicaset-d2brn
  Normal  SuccessfulCreate  61s   replicaset-controller  Created pod: nginx-replicaset-frp4d
```

Create a NodePort Service that matches the ReplicaSet pod labels:
```
vi nginx-service.yaml
```
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx_app
    type: front-end
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```
Apply it:
```
kubectl apply -f nginx-service.yaml
```
Then patch the NodePort:
```
kubectl patch svc nginx-service -p '{"spec":{"ports":[{"port":80,"targetPort":80,"nodePort":30080}]}}'
```
Verify:
```
kubectl get svc nginx-service
kubectl describe svc nginx-service
```
Expected result:
```
NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   <cluster-ip>    <none>        80:30080/TCP   ...
```
The service will route traffic to pods with labels:
```
app=nginx_app
type=front-end
```
and expose the application on NodePort 30080.
