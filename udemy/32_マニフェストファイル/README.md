# 32_マニフェストファイル

pod.yml

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
    env: study
spec:
  containers:
    - name: nginx
      image: nginx:1.17.2-alpine
```

# example
```
# kubectl apply -f pod.yml
pod/nginx created
# kubectl get pod
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          119s
# kubectl delete -f pod.yml
pod "nginx" deleted
# kubectl get pod
No resources found.
```
