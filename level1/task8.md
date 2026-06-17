The Nautilus DevOps team is setting up recurring tasks on different schedules. Currently, they're developing scripts to be executed periodically. To kickstart the process, they're creating cron jobs in the Kubernetes cluster with placeholder commands. Follow the instructions below:



Create a cronjob named devops.


Set Its schedule to something like */5 * * * *. You can set any schedule for now.


Name the container cron-devops.


Utilize the httpd image with latest tag (specify as httpd:latest).


Execute the dummy command echo Welcome to xfusioncorp!.


Ensure the restart policy is OnFailure.


Note: The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.

Create the CronJob using the following manifest:
```
apiVersion: batch/v1
kind: CronJob
metadata:
  name: devops
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
          - name: cron-devops
            image: httpd:latest
            command:
            - /bin/sh
            - -c
            - echo Welcome to xfusioncorp!
```
Save it as devops-cronjob.yaml and apply it:
```
kubectl apply -f devops-cronjob.yaml
```
Verify the CronJob:
```
kubectl get cronjob devops
kubectl describe cronjob devops
```
