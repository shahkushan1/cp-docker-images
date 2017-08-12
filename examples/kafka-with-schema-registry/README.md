# Testing that the setup works

### Testing Kafka

```bash
kafka-topics --create --topic foo --partitions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:32181
kafka-topics --describe --topic foo --zookeeper zookeeper:32181
bash -c "seq 42 | kafka-console-producer --request-required-acks 1 --broker-list localhost:29092 --topic foo && echo 'Produced 42 messages.'"
kafka-console-consumer --bootstrap-server localhost:29092 --topic foo --new-consumer --from-beginning --max-messages 42
```

### Testing Schema-Registry

```bash
/usr/bin/kafka-avro-console-producer \
  --broker-list kafka:29092 --topic bar \
  --property value.schema='{"type":"record","name":"myrecord","fields":[{"name":"f1","type":"string"}]}'

{"f1": "value1"}
{"f1": "value2"}
{"f1": "value3"}

```

Known issues -

1. The schema registry avro producer doesn't work when invoked for the first time but works correctly
during next invocations