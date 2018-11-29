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



### Here is how to test the ingress controller.

```[user@phatbox templates (⎈ |Epick8s:default)]$ kubectl get services
NAME                       TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)           AGE
echoheaders-default        NodePort       10.0.232.68    <none>           80:30302/TCP      24m
helloworld-nginx-service   LoadBalancer   10.0.247.101   168.61.221.101   80:32313/TCP      39m
helloworld-service         NodePort       10.0.101.187   <none>           31001:31001/TCP   1h
helloworld-v1              NodePort       10.0.72.188    <none>           80:30303/TCP      24m
helloworld-v2              NodePort       10.0.60.3      <none>           80:30304/TCP      24m
kubernetes                 ClusterIP      10.0.0.1       <none>           443/TCP           4h
[user@phatbox templates (⎈ |Epick8s:default)]$ curl -k --verbose --header 'Host: helloworld-v2.ghettolabs.io' 'http://168.61.221.101'
* Rebuilt URL to: http://168.61.221.101/
*   Trying 168.61.221.101...
* TCP_NODELAY set
* Connected to 168.61.221.101 (168.61.221.101) port 80 (#0)
> GET / HTTP/1.1
> Host: helloworld-v2.ghettolabs.io
> User-Agent: curl/7.61.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Date: Thu, 29 Nov 2018 21:08:50 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 12
< Connection: keep-alive
< X-Powered-By: Express
< ETag: W/"c-Lve95gjOVATpfV8EL5X4nxwjKHE"
< 
* Connection #0 to host 168.61.221.101 left intact
Hello World!

```
