# Ingress Controllers.

Ingress controllers are the most popular deploy. They are a little different depending on where you are deploying.



### Azure
helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2

### AWS

### GCP

(Creates a LoadBalancer)
helm install --name nginx-ingress stable/nginx-ingress --set rbac.create=true


### On-prem

When you deploy on-prem you will want to deploy as a daemon set.
The nginx controller in this repo works for this case.
