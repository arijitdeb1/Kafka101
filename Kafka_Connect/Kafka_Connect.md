# Kafka Connect

![ScreenShot](/images/kafka-connect-arch.PNG?raw=true)

We can setup Kafka Connect in two modes:
   
    - Standalone Mode
    - Distributed Mode

## Standalone Mode
- In standalone mode, Kafka Connect runs as a single process/worker.
- It is useful for development and testing.
- Need to create a worker.properties file to configure the broker details.
- Need to create a connector.properties file to configure the connector details.

## Distributed Mode
- In distributed mode, Kafka Connect runs as a cluster of workers.
- It is useful for production.
- No need to create a worker.properties file.
- Configuration is submitted using REST API.


## Sample Connector Configuration for Standalone Mode
1. `file` as a source [here]()


## Useful Links-
1. For more details on Kafka Connect, refer to the [official documentation](https://docs.confluent.io/current/connect/index.html).
2. For more connectors, refer to the [Confluent Hub](https://www.confluent.io/hub/).

