
# pod 作成
	# kubectl apply -f pod.yml
	pod/test created

# pod 一覧
	# kubectl get -f pod.yml
	NAME   READY   STATUS      RESTARTS   AGE
	test   0/1     Completed   3          78s

	# kubectl get all
	NAME       READY   STATUS             RESTARTS   AGE
	pod/test   0/1     CrashLoopBackOff   3          98s


	NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
	service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   93d

# pod 一覧
	# kubectl get pod
	NAME   READY   STATUS             RESTARTS   AGE
	test   0/1     CrashLoopBackOff   4          2m16s

+ STATUS
+ Running： 稼働中
+ Pending： Pod起動待ち
+ ImageNotReady: dockerイメージ取得中
+ PullImageError： dockerイメージ取得失敗
+ CreatingContainer: Pod(コンテナ)起動中
+ Error: エラー
+ etc.


# pod 削除

	# kubectl delete -f pod.yml
	pod "test" deleted

	# kubectl get pod
	No resources found.
