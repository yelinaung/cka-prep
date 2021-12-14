### Etcd Cluster

- Starting the etcd cluster with [docker-compose](https://docs.docker.com/compose/).

```bash
docker-compose -f etcd_docker_compose.yaml up -d
```
This will spin up three Docker containers like this.

```
➜  cka-prep git:(master) ✗ docker-compose -f etcd_docker_compose.yaml up -d
Starting cka-prep_etcd3_1 ... done
Starting cka-prep_etcd1_1 ... done
Starting cka-prep_etcd2_1 ... done
➜  cka-prep git:(master) ✗ docker ps
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS        PORTS                                                 NAMES
e172e4c8c297   bitnami/etcd:3   "/opt/bitnami/script…"   6 minutes ago    Up 1 second   2380/tcp, 0.0.0.0:3379->2379/tcp, :::3379->2379/tcp   cka-prep_etcd2_1
ddd26b9cd916   bitnami/etcd:3   "/opt/bitnami/script…"   56 minutes ago   Up 1 second   0.0.0.0:2379->2379/tcp, :::2379->2379/tcp, 2380/tcp   cka-prep_etcd1_1
1567c84a8ef7   bitnami/etcd:3   "/opt/bitnami/script…"   56 minutes ago   Up 1 second   2380/tcp, 0.0.0.0:4379->2379/tcp, :::4379->2379/tcp   cka-prep_etcd3_1
```

- Download etcd from [Github release page](https://github.com/etcd-io/etcd/releases). And put `etcdctl` into `$PATH`.
- Set etcd endpoints by 

```bash
$ export ENDPOINTS=127.0.0.1:2379,127.0.0.1:3379,127.0.0.1:4379
```

- Check the status of the nodes. 

```bash
$ etcdctl --endpoints=$ENDPOINTS member list --write-out=table
+------------------+---------+-------+-------------------+-------------------+------------+
|        ID        | STATUS  | NAME  |    PEER ADDRS     |   CLIENT ADDRS    | IS LEARNER |
+------------------+---------+-------+-------------------+-------------------+------------+
| 537c1a47765e155b | started | etcd2 | http://etcd2:3380 | http://etcd2:3379 |      false |
| a7cffa9ef499df5f | started | etcd3 | http://etcd3:4380 | http://etcd3:4379 |      false |
| ade526d28b1f92f7 | started | etcd1 | http://etcd1:2380 | http://etcd1:2379 |      false |
+------------------+---------+-------+-------------------+-------------------+------------+
```

- Try adding some values.

```bash
$ etcdctl --endpoints=$ENDPOINTS put mykey myvalue
```

- Get it back.
```bash
$ etcdctl --endpoints=$ENDPOINTS get mykey
mykey
myvalue
```
