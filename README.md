# kube-cron-example
This is meant to serve as an example for running cron jobs inside of a pod on Kubernetes. Example image from https://github.com/bambash/docker-cron. Its fine for simple things, but not recommended to run crons as root.

To use this deployment, add your cronfiles to Cronjobs and create a ConfigMap:
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
