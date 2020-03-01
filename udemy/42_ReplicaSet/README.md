# ReplicaSet作成

1. ReplicaSetマニフェストファイル作成
2. リソース作成
3. 手動スケールアウト

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
      env: study
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

## ReplicaSet 

1. ReplicaSetマニフェストファイル作成

2. リソース作成

```
    [root@localhost 42_ReplicaSet]# kubectl apply -f replicaset.yml
    replicaset.apps/nginx configured
    [root@localhost 42_ReplicaSet]# kubectl get all
    NAME              READY   STATUS    RESTARTS   AGE
    pod/nginx-6scnr   1/1     Running   0          72s
    pod/nginx-rbb7q   1/1     Running   0          5s
    pod/nginx-rgdbh   1/1     Running   0          5s
    pod/nginx-spjgr   1/1     Running   0          72s
    
    
    NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
    service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   61m
```

3. 手動スケールアウト

replicas の数を 増やしたりしてみる

念のため、コンテナに入ってみる

    [root@localhost 42_ReplicaSet]# kubectl exec -it pod/nginx-rgdbh sh
    / #
    / # ip -f inet a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
    2790: eth0@if2791: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
        inet 172.17.0.8/16 brd 172.17.255.255 scope global eth0
           valid_lft forever preferred_lft forever
    / # exit
    [root@localhost 42_ReplicaSet]# kubectl exec -it pod/nginx-rbb7q sh
    / # ip -f inet a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
    2788: eth0@if2789: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
        inet 172.17.0.7/16 brd 172.17.255.255 scope global eth0
           valid_lft forever preferred_lft forever
    / # exit


4.お片付け

    [root@localhost 42_ReplicaSet]# kubectl delete -f replicaset.yml
    replicaset.apps "nginx" deleted
    [root@localhost 42_ReplicaSet]# kubectl get all
    NAME              READY   STATUS        RESTARTS   AGE
    pod/nginx-6scnr   0/1     Terminating   0          3m12s
    pod/nginx-rbb7q   0/1     Terminating   0          2m5s
    pod/nginx-rgdbh   0/1     Terminating   0          2m5s
    pod/nginx-spjgr   0/1     Terminating   0          3m12s

    NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
    service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   63m

