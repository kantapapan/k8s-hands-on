# Pod作成

- 1. ホストにフォルダ、ファイルを作成
- 2. 作成したフォルダをマウントしたPodマニフェストファイルを作成
- 3. リソース作成

## ManifestFile

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sample
spec:
  containers:
  - name: nginx
    image: nginx:1.17.2-alpine
    volumeMounts:
    - name: storage
      mountPath: /home/nginx
  volumes:
  - name: storage
    hostPath:
      path: "/data/storage"
      type: Directory
```

```
# echo "hello world" >> /data/storage/hello.txt
# cat /data/storage/hello.txt
hello world
```

```
# kubectl apply -f pod.yml
pod/sample created
# kubectl get pod
NAME     READY   STATUS    RESTARTS   AGE
sample   1/1     Running   0          8s
```

```
# kubectl exec -it sample sh
/ # ls /home/nginx/
hello.txt
/ # cat /home/nginx/
cat: read error: Is a directory
/ # cat /home/nginx/hello.txt
hello world
/ # exit
```

```
# kubectl delete -f pod.yml
pod "sample" deleted
# kubectl delete -f pod.yml
```
