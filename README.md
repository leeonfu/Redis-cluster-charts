# redis-cluster-helm

## 1、Requirements Helm 3.0+ and NFS Server

helm install redis-cluster redis-cluster-helm

Join the cluster

```shell
export redis_cluster_ip=$(kubectl get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 '|awk  '{for(i=1;i<=6;i++){printf $i;printf " "}}')
kubectl exec -it redis-cluster-0 -- redis-cli -p 6379 --cluster create --cluster-replicas 1 ${redis_cluster_ip}
```

Get the cluster information details.

```shell
[root@kube-node01 ~]# kubectl exec -it redis-cluster-0 -- redis-cli cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:62604
cluster_stats_messages_pong_sent:59158
cluster_stats_messages_sent:121762
cluster_stats_messages_ping_received:59153
cluster_stats_messages_pong_received:62604
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:12176
```

```shell
[root@kube-node01 ~]# kubectl exec -it redis-cluster-0 -- redis-cli cluster nodes
37607e3e966c50a73dfad5bc90d79e5eeb66d428 172.20.13.87:6379@16379 slave de9f751530518b7dce79c2497703eed850202430 0 1622170723365 3 connected
de9f751530518b7dce79c2497703eed850202430 172.20.0.143:6379@16379 master - 0 1622170723000 3 connected 10923-16383
625eaba23941fe3677657f87cada1479ccd5dcfc 172.20.237.82:6379@16379 master - 0 1622170722000 2 connected 5461-10922
2465913762a839ed93b9d5149b14369391501ead 172.20.238.14:6379@16379 slave 4b564d2072bac743ee6abe5078933b2723c7ce54 0 1622170721553 1 connected
c58ef1a1a7a3819447b9c05f5612ef3080c74010 172.20.161.15:6379@16379 slave 625eaba23941fe3677657f87cada1479ccd5dcfc 0 1622170720346 2 connected
4b564d2072bac743ee6abe5078933b2723c7ce54 172.20.172.214:6379@16379 myself,master - 0 1622170719000 1 connected 0-5460

```

