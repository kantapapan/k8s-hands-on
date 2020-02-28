# 33_マニフェストファイル2

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: debug
  namespace: default
spec:
  containers:
    - name: debug
      image: centos:7
      command:
        - "sh"
        - "-c"
      args:
        - |
          while true
          do
            sleep ${DELAY}
          done
      env:
        - name: "DELAY"
          value: "5"
```

# example

```
# pod delete

```
# kubectl apply -f pod.yml
pod/debug created
# kubectl get pod
NAME    READY   STATUS              RESTARTS   AGE
debug   0/1     ContainerCreating   0          8s
# kubectl get pod -w
NAME    READY   STATUS              RESTARTS   AGE
debug   0/1     ContainerCreating   0          16s
# kubectl get pod
NAME    READY   STATUS    RESTARTS   AGE
debug   1/1     Running   0          67s
```


## docker exec 

	# docker ps
	CONTAINER ID        IMAGE                                                            COMMAND                   CREATED              STATUS              PORTS                                                                NAMES
	6ac44c8ccdeb        centos                                                           "sh -c 'while true\nd…"   42 seconds ago       Up 40 seconds                                                                            k8s_debug_debug_default_1dea105a-9cbf-4082-9db4-4addb625d910_0
	# docker exec -it 6ac44c8ccdeb sh
	sh-4.2# cat /etc/redhat-release
	CentOS Linux release 7.7.1908 (Core)
	sh-4.2# exit


# pod delete

	# kubectl delete -f pod.yml
	pod "debug" deleted
	# kubectl get pod
	No resources found.


