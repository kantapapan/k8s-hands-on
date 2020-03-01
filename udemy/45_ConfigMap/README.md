# ConfigMap作成

- k8s上で管理する設定情報
- data にキーバリュー形式で管理


## 内容

1. ConfigMapとPodを含むマニフェストファイル作成
2. リソース作成
3. Podに入ってConfigMapが接続されていることを確認

## Manifestfile

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: sample-config
  namespace: default
data:
  sample.cfg: |
    user: romame.d
  type: "application"

---
apiVersion: v1
kind: Pod
metadata:
  name: sample
  namespace: default
spec:
  containers:
  - name: sample
    image: nginx:1.17.2-alpine
    env:
    - name: TYPE
      valueFrom:
        configMapKeyRef:
          name: sample-config
          key: type
    volumeMounts:
    - name: config-storage
      mountPath: /home/nginx
  volumes:
  - name: config-storage
    configMap:
      name: sample-config
      items:
      - key: sample.cfg
        path: sample.cfg
```

# example

+ リソース作成

```
[root@localhost 45_ConfigMap]# kubectl apply -f configmap.yml
configmap/sample-config created
pod/sample created
[root@localhost 45_ConfigMap]# kubectl get pod
NAME     READY   STATUS    RESTARTS   AGE
sample   1/1     Running   0          7s
[root@localhost 45_ConfigMap]# kubectl get all
NAME         READY   STATUS    RESTARTS   AGE
pod/sample   1/1     Running   0          13s


NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5h23m

```

+ コンテナに入る

```
[root@localhost 45_ConfigMap]# kubectl exec -it sample sh
/ # ls -l /home/nginx/
total 0
lrwxrwxrwx    1 root     root            17 Mar  1 10:30 sample.cfg -> ..data/sample.cfg  << 作られている

/ # cat /home/nginx/sample.cfg
user: romame.d


/ # env
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_SERVICE_PORT=443
HOSTNAME=sample
SHLVL=1
HOME=/root
PKG_RELEASE=1
TERM=xterm
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
NGINX_VERSION=1.17.2
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
NJS_VERSION=0.3.3
KUBERNETES_PORT_443_TCP_PROTO=tcp
TYPE=application               << できている
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_SERVICE_HOST=10.96.0.1
PWD=/
/ # exit
```

+ お片付け
```
[root@localhost 45_ConfigMap]# kubectl delete -f configmap.yml
configmap "sample-config" deleted
pod "sample" deleted
```

