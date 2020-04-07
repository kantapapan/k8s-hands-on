# 永続データ作成
1. PVとPVCを含むマニフェストファイル作成
2. リソース作成

# 実行

ホストのstorageを作る
```
[root@localhost 47_PersistentVolume_PersistentVolumeClaim]# mkdir /data/storage
[root@localhost 47_PersistentVolume_PersistentVolumeClaim]# touch /data/storage/a.txt
```

## リソースを作る

```
[root@localhost 47_PersistentVolume_PersistentVolumeClaim]# kubectl get pvc,pv
NAME                                 STATUS   VOLUME      CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/volume-claim   Bound    volume-01   1Gi        RWO            slow           24s

NAME                         CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   REASON   AGE
persistentvolume/volume-01   1Gi        RWO            Retain           Bound    default/volume-claim   slow                    24s

```

## 片付け

```
[root@localhost 47_PersistentVolume_PersistentVolumeClaim]# kubectl delete -f storage.yml
```

