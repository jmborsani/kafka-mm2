# kafka-mm2

### kafka-connect 

Os workers devem ter os mesmos valores para subir um cluster:

- group.id
- config.storage.topic
- offset.storage.topic
- status.storage.topic

Exemplo:

```conf
bootstrap.servers=kafka-target-0:29094,kafka-target-1:29095

group.id=connect-cluster

key.converter=org.apache.kafka.connect.json.JsonConverter
value.converter=org.apache.kafka.connect.json.JsonConverter
key.converter.schemas.enable=true
value.converter.schemas.enable=true

internal.key.converter=org.apache.kafka.connect.json.JsonConverter
internal.value.converter=org.apache.kafka.connect.json.JsonConverter
internal.key.converter.schemas.enable=false
internal.value.converter.schemas.enable=false

offset.storage.topic=connect-offsets
offset.storage.replication.factor=1
#offset.storage.partitions=25

config.storage.topic=connect-configs
config.storage.replication.factor=1

status.storage.topic=connect-status
status.storage.replication.factor=1
# status.storage.partitions=5

offset.flush.interval.ms=10000

rest.port=8083
```

Doc de configuração do Kafka-connect 

https://docs.confluent.io/platform/current/connect/userguide.html#distributed-mode
https://docs.confluent.io/platform/7.6/connect/references/allconfigs.html#distributed-worker-configuration

Exemplos:

https://github.com/aws-samples/kafka-connect-mm2
https://github.com/aws-samples/mirrormaker2-msk-migration

### Connectors

- MirrorCheckpointConnector
- MirrorHeartbeatConnector
- MirrorSourceConnector

Atenção: desligar o sync automatico de acls e topicos para evitar que o connector altere as configurações dos tópicos:

Criar os tópicos internos:

```shell
# config.storage.topic=connect-configs
bin/kafka-topics --create --bootstrap-server localhost:9092 --topic connect-configs --replication-factor 3 --partitions 1 --config cleanup.policy=compact
```

```shell
# offset.storage.topic=connect-offsets
bin/kafka-topics --create --bootstrap-server localhost:9092 --topic connect-offsets --replication-factor 3 --partitions 50 --config cleanup.policy=compact
```

```shell
# status.storage.topic=connect-status
bin/kafka-topics --create --bootstrap-server localhost:9092 --topic connect-status --replication-factor 3 --partitions 10 --config cleanup.policy=compact
```

### jmx-exporter

Download `jmx_prometheus_javaagent-0.20.0.jar` -> https://github.com/prometheus/jmx_exporter/releases

Config:
- kafka-connect: https://github.com/prometheus/jmx_exporter/blob/main/example_configs/kafka-connect.yml
- kafka: https://github.com/prometheus/jmx_exporter/blob/main/example_configs/kafka-2_0_0.yml


### prometheus


### Grafana


#### kafka-ui

[kafka-ui](./assets/kafka-ui.png)


# Referência 

Poc inspirada na doc -> https://medium.com/@penkov.vladimir/running-mirror-maker-2-on-kafka-connect-cluster-3b391d686efe