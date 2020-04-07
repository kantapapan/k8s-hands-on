# Secret作成

1. SecretとPodを含むマニフェストファイル作成
2. リソース作成
3. Podに入ってSecretが接続されていることを確認


## pod 作成

```
[root@localhost 46_Secret]# kubectl apply -f secret.yml 
secret/sample-secret created
pod/sample created

[root@localhost 46_Secret]# kubectl get pod
NAME     READY   STATUS    RESTARTS   AGE
sample   1/1     Running   0          20s
```


## podに入って確認していく


```
[root@localhost 46_Secret]# kubectl exec -it sample sh
```

- ファイルから確認
  base64 エンコードしたものが展開されていることを確認
```
/ # ls /home/nginx/
keyfile
/ # cat /home/nginx/keyfile 
YOUR-SECRET-KEY/ # 
```


環境変数のほうも確認
```
/ # env
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.96.0.1:443
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
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
MESSAGE=Hello World !
KUBERNETES_SERVICE_HOST=10.96.0.1
PWD=/

/ # exit
```

## 片付け

```
[root@localhost 46_Secret]# kubectl delete -f secret.yml 
secret "sample-secret" deleted
pod "sample" deleted
```

