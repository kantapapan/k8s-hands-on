# pod 転送

```
# kubectl apply -f pod.yml
pod/debug created
```

```
# kubectl get pod
NAME    READY   STATUS    RESTARTS   AGE
debug   1/1     Running   0          7s
```

```
# kubectl cp ./transfer.txt debug:/var/tmp/transfer.txt
```

```
# kubectl exec -it debug sh
sh-4.2#
sh-4.2# cat /var/tmp/transfer.txt
aaaa
sh-4.2#
sh-4.2# pwd
/
sh-4.2# echo "bbb" >> /var/tmp/transfer2.txt
sh-4.2# cat /var/tmp/transfer2.txt
bbb
sh-4.2# exit
exit
```

```
# kubectl get pod
NAME    READY   STATUS    RESTARTS   AGE
debug   1/1     Running   0          7m27s
```

```
# kubectl cp debug:/var/tmp/transfer2.txt ./transfer2.txt
tar: Removing leading `/' from member names
```

```
# ll
合計 12
-rw-r--r--. 1 root root 360  2月 29 02:34 pod.yml
-rw-r--r--. 1 root root   5  2月 29 02:35 transfer.txt
-rw-r--r--. 1 root root   4  2月 29 02:43 transfer2.txt
```

```
# cat transfer2.txt
bbb
```

```
# kubectl delete -f pod.yml
```

