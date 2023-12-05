## Docker篇

### zookeeper启动

> kafka依赖zookeeper


```bash
docker run -it --name zookeeper --net hacker -p 2181:2181 -e ZOO_MY_ID=1  zookeeper
```

### kafka启动

```bash
docker run -it --name kafka --net hacker -p 9092:9092 -p 9094:9094 -e KAFKA_CFG_LISTENERS=PLAINTEXT://:9092,CONTROLLER://:9093,EXTERNAL://:9094 -e KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://kafka:9092,EXTERNAL://localhost:9094 -e KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181 -e KAFKA_CFG_LISTENER_SECURITY_PROTOCOL_MAP=CONTROLLER:PLAINTEXT,EXTERNAL:PLAINTEXT,PLAINTEXT:PLAINTEXT  bitnami/kafka
```

### 常规操作

- 创建topic

```bash
bin/kafka-topics.sh --create --topic logs --bootstrap-server kafka:9092
```

- 查看主题列表

```bash
bin/kafka-topics.sh --list --bootstrap-server kafka:9092
```

- 阅读事件

```bash
bin/kafka-console-consumer.sh --topic logs --from-beginning --bootstrap-server kafka:9092
```

- 写入主题

```bash
bin/kafka-console-producer.sh --topic logs --bootstrap-server kafka:9092
```
