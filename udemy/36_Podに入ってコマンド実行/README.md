# podに入ってコマンド実行



	# kubectl apply -f pod.yml
	pod/debug created
	pod/nginx created


	# kubectl get pod -o wide
	NAME    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
	debug   1/1     Running   0          48s   172.17.0.8   minikube   <none>           <none>
	nginx   1/1     Running   0          48s   172.17.0.9   minikube   <none>           <none>


	# kubectl exec -it debug sh
	sh-4.2# curl 172.17.0.9
	<!DOCTYPE html>
	<html>
	<head>
	<title>Welcome to nginx!</title>
	<style>
	    body {
		width: 35em;
		margin: 0 auto;
		font-family: Tahoma, Verdana, Arial, sans-serif;
	    }
	</style>
	</head>
	<body>
	<h1>Welcome to nginx!</h1>
	<p>If you see this page, the nginx web server is successfully installed and
	working. Further configuration is required.</p>

	<p>For online documentation and support please refer to
	<a href="http://nginx.org/">nginx.org</a>.<br/>
	Commercial support is available at
	<a href="http://nginx.com/">nginx.com</a>.</p>

	<p><em>Thank you for using nginx.</em></p>
	</body>
	</html>


	sh-4.2# exec attach failed: error on attach stdin: read escape sequence
	command terminated with exit code 126


	# kubectl get pod -o wide
	NAME    READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
	debug   1/1     Running   0          4m34s   172.17.0.8   minikube   <none>           <none>
	nginx   1/1     Running   0          4m34s   172.17.0.9   minikube   <none>           <none>

	# kubectl delete -f pod.yml
	pod "debug" deleted
	pod "nginx" deleted
