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