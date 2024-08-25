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
1. `file` as a source [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/File/File_Connectors_Standalone.md)

## Sample Connector Configuration for Distributed Mode
1. `file` as a source [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/File/File_Connectors_Distributed.md)
2. `database` as a source [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/Database/DB_Connectors_Distributed.md)
3. `elasticsearch` as a sink [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/ElasticSearch/ES_Connectors_Distributed.md)

## REST API for Kafka Connect
** Install jq to parse the JSON response. Execute the below command to install jq.
```docker run --rm -it --net=host landoop/fast-data-dev:cp3.3.0 bash
   apt-get update
   apt-get install jq
```

1. **Get worker info** 
     
       curl -s http://127.0.0.1:8083/ | jq

2. **List connectors available on a worker**
   
       curl -s http://127.0.0.1:8083/connector-plugins | jq

3. **Ask about active connectors**
 
       curl -s http://127.0.0.1:8083/connectors | jq

4. **Get information about a connector task and config** 

       curl -s http://127.0.0.1:8083/connectors/source-postgres-distributed/tasks | jq

5. **Get Connector status**

       curl -s http://127.0.0.1:8083/connectors/sink-elastic-distributed/status | jq

6. **Pause/Resume a Connector**

       curl -s http://127.0.0.1:8083/connectors/sink-elastic-distributed/pause
       curl -s http://127.0.0.1:8083/connectors/sink-elastic-distributed/resume

7. **Delete a Connector**
 
       curl -s -X DELETE http://127.0.0.1:8083/connectors/sink-elastic-distributed/

8. **Create a new connector**
  
       curl -X POST http://127.0.0.1:8080/connectors -H "Content-Type: application/json" -d '{
       "name": "<name of connector>",
       "config": {
          "<key>": "<value>",
          "<key>": "<value>",
          "<key>": "<value>"          
        }
       }'

9. **Update a connector**

       curl -X PUT http://127.0.0.1:8080/connectors -H "Content-Type: application/json" -d '{
       "name": "<name of connector>",
       "config": {
          "<key>": "<value>",
          "<key>": "<value>",
          "<key>": "<value>"          
        }
       }'

     
    
## Useful Links-
1. For more details on Kafka Connect, refer to the [official documentation](https://docs.confluent.io/current/connect/index.html).
2. For more connectors, refer to the [Confluent Hub](https://www.confluent.io/hub/).

