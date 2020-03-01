# Deployment作成

1. Deploymentマニフェストファイル作成
2. リソース作成
3. ロールアウト履歴確認
4. Deployment修正
5. ロールアウト履歴確認
6. ロールバック


# Manifestfile

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  annotations:
    kubernetes.io/change-cause: "First release."
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
      env: study
  revisionHistoryLimit: 14
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  template:
    metadata:
      name: nginx
      labels:
        app: web
        env: study
    spec:
      containers:
      - name: nginx
        image: nginx:1.17.2-alpine
```

# example

## create resource from deployment01.yml

```
[root@localhost 43_Deployment]# kubectl apply -f deployment01.yml
deployment.apps/nginx created
```

## get all
```
[root@localhost 43_Deployment]# kubectl get all
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-5ccc9865bd-hhzq9   1/1     Running   0          22s
pod/nginx-5ccc9865bd-qcpzn   1/1     Running   0          22s


NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   128m


NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   2/2     2            2           22s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-5ccc9865bd   2         2         2       22s
```

## rollout ヒストリーを確認

```
[root@localhost 43_Deployment]# kubectl rollout history deploy/nginx
deployment.extensions/nginx 
REVISION  CHANGE-CAUSE
1         First release.
```

## update resource from deployment02.yml

```
[root@localhost 43_Deployment]# kubectl apply -f deployment02.yml 
deployment.apps/nginx configured
[root@localhost 43_Deployment]# 
[root@localhost 43_Deployment]# kubectl rollout history deploy/nginx
deployment.extensions/nginx 
REVISION  CHANGE-CAUSE
1         First release.
2         Update nginx:1.17.3.  <<< ちゃんとアップデートされた

[root@localhost 43_Deployment]# kubectl get all
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-6d8b546b45-kkjqt   1/1     Running   0          32s
pod/nginx-6d8b546b45-kxsql   1/1     Running   0          32s


NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   133m


NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   2/2     2            2           5m9s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-5ccc9865bd   0         0         0       5m9s
replicaset.apps/nginx-6d8b546b45   2         2         2       32s

```

## rollback してみる  Undo

```
[root@localhost 43_Deployment]# kubectl get all
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-5ccc9865bd-qbkcl   1/1     Running   0          27s  << AGEが若くなっているので更新されている
pod/nginx-5ccc9865bd-sdxcs   1/1     Running   0          27s  << 


NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   135m


NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   2/2     2            2           7m13s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-5ccc9865bd   2         2         2       7m13s
replicaset.apps/nginx-6d8b546b45   0         0         0       2m36s
```

## ヒストリーを確認

```
[root@localhost 43_Deployment]# kubectl rollout history deploy/nginx
deployment.extensions/nginx
REVISION  CHANGE-CAUSE
2         Update nginx:1.17.3.
3         First release.  << ちゃんと FirstReleaseになっているぞ
```

## お片付け

```
[root@localhost 43_Deployment]# kubectl delete -f deployment01.yml
```





