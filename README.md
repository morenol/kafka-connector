# Fluvio Kafka Connectors
Official Infinyon Kafka connectors 

## Sink Connector
This is a connector for taking data from a Fluvio topic and sending to a Kafka topic.

See [docs](https://www.fluvio.io/connectors/outbound/kafka/) here.

### Configuration

| Opt            | default               | type     | description                            |
| :---           | :---                  | :---     | :----                                  |
| url            | -                     | String   | The url for the kafka connector        |
| topic          | -                     | String   | The kafka topic                        |
| partition      | 0                     | Integer  | The kafka partition                    |
| create-topic   | false                 | Boolean  | Create or not a topic before start     |
| options        | -                     | Mapping  | The kafka client options               |
| security       | -                     | Mapping  | Optional. The kafka security config    |

#### Security configuration
| Option               | default  | type     | description                            |
| :---                 | :---     | :---     | :----                                  |
| security_protocol    | ssl      | String   | The kafka security protocol            |
| ssl_key              | -        | Mapping  | The SSL key file to use                |
| ssl_cert             | -        | Mapping  | The SSL cert file to use               |
| ssl_ca               | -        | Mapping  | The SSL ca file to use                 |

Parameters `ssl_key`, `ssl_cert` and `ssl_ca` can be defined via `file` - path to the file, or `pem` - content as string value.

Example without security:
```yaml
meta:
  version: latest
  name: my-kafka-connector
  type: kafka-sink
  topic: kafka-topic
  create-topic: true
kafka:
  url: "localhost:9092"
  topic: fluvio-topic 
  create-topic: true
```

Example with security enabled:
```yaml
meta:
  version: latest
  name: my-kafka-connector
  type: kafka-sink
  topic: kafka-topic
  create-topic: true
kafka:
  url: "KAFKA_BROKER_URL"
  topic: fluvio-topic 
  create-topic: true
  security:
    ssl_key:
      file: /path/to/file
    ssl_cert:
      pem: "CONTENT IN PEM FORMAT"
    ssl_ca:
      file: /path/to/file
    security_protocol: ssl
```

### Usage
To try out Kafka Sink connector locally, you can use Fluvio CDK tool:
```bash
fluvio install cdk

cdk deploy -p kafka-sink start --config crates/kafka-sink/config-example.yaml
```


## Source Connector
This is a connector for taking data from a Kafka topic and sending to a Fluvio topic.

See [docs](https://www.fluvio.io/connectors/inbound/kafka/) here.

### Configuration

| Opt            | default               | type     | description                            |
| :---           | :---                  | :---     | :----                                  |
| url            | -                     | String   | The url for the kafka connector        |
| topic          | -                     | String   | The kafka topic                        |
| partition      | 0                     | Integer  | The kafka partition                    |
| group          | fluvio-kafka-source   | String   | The kafka consumer group               |

Example:
```yaml
meta:
  version: latest
  name: my-kafka-connector
  type: kafka-source
  topic: kafka-topic
  create-topic: true
kafka:
  url: "localhost:9092"
  topic: fluvio-topic 
```

### Usage
To try out Kafka Sink connector locally, you can use Fluvio CDK tool:
```bash
fluvio install cdk

cdk deploy -p kafka-source start --config crates/kafka-source/config-example.yaml
```


## Transformations
Fluvio Kafka Connectors support [Transformations](https://www.fluvio.io/docs/concepts/transformations-chain/).
