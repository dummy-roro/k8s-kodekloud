The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:


Create a deployment named nginx to deploy the application nginx using the image nginx:latest (ensure to specify the tag)

Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.
👉 Your task: Deploy an nginx application using a Kubernetes deployment.

🔹 Step 1: Connect to Jump Host
ssh thor@jumphost
Purpose: Access the jump host where kubectl is configured.

🔹 Step 2: Create Deployment Manifest
vi nginx-deployment.yaml
Purpose: Create a YAML file to define the deployment configuration.

YAML Content:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          imagePullPolicy: IfNotPresent
      restartPolicy: Always
Save and Exit: Press Esc, type :wq, press Enter.

Command Breakdown:

apiVersion: apps/v1: Uses the apps/v1 API for deployments.
kind: Deployment: Defines a deployment resource.
metadata.name: Sets deployment name to nginx.
spec.replicas: Sets one replica (adjustable).
spec.selector: Matches pods with app: nginx label.
spec.template: Defines the pod template with container nginx and image nginx:latest.
imagePullPolicy: IfNotPresent: Optimizes image pulling.
restartPolicy: Always: Ensures pod restarts on failure.
🔹 Step 3: Apply the Deployment Manifest
kubectl apply -f nginx-deployment.yaml
Purpose: Deploy the nginx application to the cluster.

Expected Output:

deployment.apps/nginx created
🔹 Step 4: Verify Deployment
kubectl get deployments
Purpose: Confirm the deployment is created and running.

Expected Output:

NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           10s
Verify Pods:

kubectl get pods -l app=nginx
Expected Output:

NAME                     READY   STATUS    RESTARTS   AGE
nginx-xxx-xxx   1/1     Running   0          10s
🔹 Step 5: Verify Deployment Details
kubectl describe deployment nginx
Purpose: Check deployment details, including labels and container status.

Expected Output (partial):

Name:                   nginx
Selector:               app=nginx
Replicas:               1 desired | 1 updated | 1 total
Containers:
  nginx:
    Image:        nginx:latest
    State:        Running
📋 Quick Command Reference
# Create and apply deployment manifest
vi nginx-deployment.yaml
# (Paste YAML content)
kubectl apply -f nginx-deployment.yaml

# Verify deployment and pods
kubectl get deployments
kubectl get pods -l app=nginx
kubectl describe deployment nginx
💡 Additional Tips
Task Variability: Deployment names or namespaces may differ; adapt the YAML accordingly.
Replicas: Default is 1 replica; scale as needed with kubectl scale.
Labels: Consistent labeling (app: nginx) ensures proper selector matching.
Dry Run: Use kubectl apply -f nginx-deployment.yaml --dry-run=client to test.
