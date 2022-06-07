- single-zookeeper-docker-compose.yaml 3kf, 1zk, Working.
- 3zk-3kf-docker-compose.yaml - 3kf, 3zk. Working.
- docker-compose.yaml - volumes for data/logs, 3zk, 3kf. Not working.

Useful links:
https://github.com/conduktor/kafka-stack-docker-compose
https://github.com/confluentinc/cp-docker-images/blob/5.3.3-post/examples/kafka-cluster/docker-compose.yml
https://github.com/bitnami/bitnami-docker-zookeeper/blob/master/docker-compose-cluster.yml

Guide for docker-compose.yaml:
### 1. SSH to VM and prepare env
**Create required directories:**
```
mkdir kafka && cd kafka
mkdir zk1 zk2 zk3 kf1 kf2 kf3
```
**Copy docker-compose**
```
vi docker-compose.yaml
```
**Start docker compose**
```
docker-compose up -d
```
**Get docker network private IPs of container instances**
```
docker ps -a
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name> 
```
### 2. Connect to kafka containers
```
docker exec -it <container_name> /bin/bash
```
**Broker 1 container:**
```
kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 3 --partitions 3 --topic reviews
kafka-topics --list --bootstrap-server localhost:9092
kafka-topics --bootstrap-server localhost:9092 --describe --topic reviews
kafka-console-producer --topic reviews --bootstrap-server localhost:9092
```
**Broker 2 container:**
```
kafka-console-consumer --topic reviews --from-beginning --bootstrap-server localhost:9092
```