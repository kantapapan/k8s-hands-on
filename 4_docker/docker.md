# image 取得

    docker image pull Name[:TAG]

```
[root@localhost ~]# docker image pull centos:centos7.7.1908
centos7.7.1908: Pulling from library/centos
f34b00c7da20: Pull complete
Digest: sha256:50752af5182c6cd5518e3e91d48f7ff0cba93d5d760a67ac140e2d63c4dd9efc
Status: Downloaded newer image for centos:centos7.7.1908
docker.io/library/centos:centos7.7.1908
```

# image 一覧

    docker image ls

```
[root@localhost ~]# docker image ls
REPOSITORY                                                       TAG                 IMAGE ID            CREATED             SIZE
centos                                                           centos7.7.1908      08d05d1d5859        2 weeks ago         204MB
k8s.gcr.io/kube-proxy                                            v1.15.0             d235b23c3570        5 months ago        82.4MB
k8s.gcr.io/kube-apiserver                                        v1.15.0             201c7a840312        5 months ago        207MB
k8s.gcr.io/kube-scheduler                                        v1.15.0             2d3813851e87        5 months ago        81.1MB
k8s.gcr.io/kube-controller-manager                               v1.15.0             8328bb49b652        5 months ago        159MB
quay.io/kubernetes-ingress-controller/nginx-ingress-controller   0.23.0              42d47fe0c78f        9 months ago        591MB
k8s.gcr.io/kube-addon-manager                                    v9.0                119701e77cbc        10 months ago       83.1MB
k8s.gcr.io/coredns                                               1.3.1               eb516548c180        10 months ago       40.3MB
hello-world                                                      latest              fce289e99eb9        11 months ago       1.84kB
k8s.gcr.io/etcd                                                  3.3.10              2c4adeb21b4f        12 months ago       258MB
k8s.gcr.io/heapster-amd64                                        v1.5.3              f57c75cd7b0a        19 months ago       75.3MB
k8s.gcr.io/pause                                                 3.1                 da86e6ba6ca1        23 months ago       742kB
gcr.io/k8s-minikube/storage-provisioner                          v1.8.1              4689081edb10        2 years ago         80.8MB
gcr.io/google_containers/defaultbackend                          1.4                 846921f0fe0e        2 years ago         4.84MB
k8s.gcr.io/heapster-influxdb-amd64                               v1.3.3              577260d221db        2 years ago         12.5MB
k8s.gcr.io/heapster-grafana-amd64                                v4.4.3              8cb3de219af7        2 years ago         152MB
```

# image 削除

    docker image rm IMAGE
    docker image prune

```
[root@localhost ~]# docker image rm 08d05d1d5859
Untagged: centos:centos7.7.1908
Untagged: centos@sha256:50752af5182c6cd5518e3e91d48f7ff0cba93d5d760a67ac140e2d63c4dd9efc
Deleted: sha256:08d05d1d5859ebcfb3312d246e2082e46cb307f0e896c9ac097185f0b0b19e56
Deleted: sha256:034f282942cd6c3abf9384601a57f080f8f75cc7f58527db8e07573d9d14ab46
[root@localhost ~]# docker image ls
REPOSITORY                                                       TAG                 IMAGE ID            CREATED             SIZE
k8s.gcr.io/kube-proxy                                            v1.15.0             d235b23c3570        5 months ago        82.4MB
k8s.gcr.io/kube-apiserver                                        v1.15.0             201c7a840312        5 months ago        207MB
k8s.gcr.io/kube-controller-manager                               v1.15.0             8328bb49b652        5 months ago        159MB
k8s.gcr.io/kube-scheduler                                        v1.15.0             2d3813851e87        5 months ago        81.1MB
quay.io/kubernetes-ingress-controller/nginx-ingress-controller   0.23.0              42d47fe0c78f        9 months ago        591MB
k8s.gcr.io/kube-addon-manager                                    v9.0                119701e77cbc        10 months ago       83.1MB
k8s.gcr.io/coredns                                               1.3.1               eb516548c180        10 months ago       40.3MB
hello-world                                                      latest              fce289e99eb9        11 months ago       1.84kB
k8s.gcr.io/etcd                                                  3.3.10              2c4adeb21b4f        12 months ago       258MB
k8s.gcr.io/heapster-amd64                                        v1.5.3              f57c75cd7b0a        19 months ago       75.3MB
k8s.gcr.io/pause                                                 3.1                 da86e6ba6ca1        23 months ago       742kB
gcr.io/k8s-minikube/storage-provisioner                          v1.8.1              4689081edb10        2 years ago         80.8MB
gcr.io/google_containers/defaultbackend                          1.4                 846921f0fe0e        2 years ago         4.84MB
k8s.gcr.io/heapster-influxdb-amd64                               v1.3.3              577260d221db        2 years ago         12.5MB
k8s.gcr.io/heapster-grafana-amd64                                v4.4.3              8cb3de219af7        2 years ago         152MB
```
