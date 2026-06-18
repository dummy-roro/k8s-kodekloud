We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:



The pod name is nginx-phpfpm and configmap name is nginx-config. Identify and fix the problem.


Once resolved, copy /home/thor/index.php file from the jump host to the nginx-container within the nginx document root. After this, you should be able to access the website using Website button on the top bar.


Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.

Solution
```
kubectl get all -o wide
```
Check configmap
```
kubectl describe configmap nginx-config
```
the issue is worng vlume mount path in nginx container, so we have to fix it
```
kubectl get pod nginx-phpfpm -o yaml  > /tmp/nginx.yaml
cat /tmp/nginx.yaml  |grep /usr/share/nginx/html
vi /tmp/nginx.yaml
```
Change vloume mount path from /usr/share/nginx/html to /var/www/html 

Then replace the container
```
kubectl replace -f /tmp/nginx.yaml --force
```
Copy index file
```
kubectl cp  /home/thor/index.php  nginx-phpfpm:/var/www/html -c nginx-container
kubectl exec -it nginx-phpfpm -c nginx-container  -- curl -I  http://localhost:8099
```
if u got 200, everything is fine and u can verify it via web ui. 
