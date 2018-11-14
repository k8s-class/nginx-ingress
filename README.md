# Ingress Controllers.

Ingress controllers are the most popular deploy. They are a little different depending on where you are deploying.



### Azure
(Creates a LoadBalancer if one does not already exists)
```
helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2
```

### AWS
```

        helm install --name my-ingress-controller stable/nginx-ingress \
          --set controller.kind=DaemonSet \
          --set controller.service.type=NodePort \
          --set controller.hostNetwork=true
```

### GCP

(Creates a LoadBalancer)
```
helm install --name nginx-ingress stable/nginx-ingress --set rbac.create=true
```

### On-prem

When you deploy on-prem you will want to deploy as a daemon set.
The nginx controller in this repo works for this case.

```
        helm install --name my-ingress-controller stable/nginx-ingress \
          --set controller.kind=DaemonSet \
          --set controller.service.type=NodePort \
          --set controller.hostNetwork=true
  ```
