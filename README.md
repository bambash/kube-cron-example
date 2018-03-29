# kube-cron-example
This is meant to serve as an example for running cron jobs. 

`deployment.yaml` will run cron jobs inside of a pod on Kubernetes. Example image from https://github.com/bambash/docker-cron. Its fine for simple things, but not recommended to run crons as root.

`cronjob.yaml` will create a CronJob resource in Kubernetes. Documentation: https://cloud.google.com/kubernetes-engine/docs/how-to/cronjobs

expanding on the CronJob resource, I pushed an example helm chart here: https://github.com/bambash/helm-cronjobs

---
To use `deployment.yaml`, add your cronfiles to Cronjobs and create a ConfigMap:
```
# kubectl create configmap cronfiles --from-file Cronfiles/
configmap "cronfiles" created
```
After the ConfigMap exists, create the deployment:
```
# kubectl apply -f deployment.yaml
deployment "cron" created   
```
After your cron executes, you should see the output:
```
# kubectl logs -f cron-7cf5bd99b7-bcq8g
Thu Mar 29 04:42:02 UTC 2018 hello from kubernetes
Thu Mar 29 04:43:01 UTC 2018 hello from kubernetes
Thu Mar 29 04:44:01 UTC 2018 hello from kubernetes
```
---
To use `cronjob.yaml`, create the resource:
```
kubectl apply -f cronjob.yaml
```
List your current cronjobs:
```
# kubectl get cronjob
NAME              SCHEDULE    SUSPEND   ACTIVE    LAST SCHEDULE   AGE
example-cronjob   * * * * *   False     0         40s             6m
```
List jobs spawned:
```
# kubectl get jobs
NAME                         DESIRED   SUCCESSFUL   AGE
example-cronjob-1522301640   1         1            2m
example-cronjob-1522301700   1         1            1m
example-cronjob-1522301760   1         1            26s
```
List finished job pods:
```
# kubectl get pods -a
NAME                                 READY     STATUS              RESTARTS   AGE
example-cronjob-1522301700-4p68q     0/1       Completed           0          3m
example-cronjob-1522301760-x4nwd     0/1       Completed           0          2m
example-cronjob-1522301820-5qtcs     0/1       Completed           0          1m
```
Get logs from finished job pod:
```
# kubectl logs example-cronjob-1522301820-5qtcs
i am a cronjob
```

