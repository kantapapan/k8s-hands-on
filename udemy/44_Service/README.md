# Service作成
1. NodePortのServiceマニフェストファイル作成
2. リソース作成
3. ブラウザからアクセスして動作確認

# Manifestfile

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: web
    env: study
spec:
  containers:
  - name: nginx
    image: nginx:1.17.2-alpine

---
apiVersion: v1
kind: Service
metadata:
  name: web-svc
spec:
  type: NodePort
  selector:
    app: web
    env: study
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30000

```

# example

```
[root@localhost 44_Service]# kubectl apply -f service.yml
pod/nginx created
service/web-svc created
[root@localhost 44_Service]# kubectl get pod
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          6s
[root@localhost 44_Service]# ip -f inet a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 76138sec preferred_lft 76138sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
5: br-19a0433b1532: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-19a0433b1532
       valid_lft forever preferred_lft forever
[root@localhost 44_Service]#
[root@localhost 44_Service]# kubectl delete -f service.yml
pod "nginx" deleted
service "web-svc" deleted
```

## 以下にPCブラウザからアクセスして webページが確認できていたらOK

http://192.168.56.103:30000/
