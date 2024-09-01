# Kafka Connect configurations to set ElasticSearch as source/sink

## To set Elasticsearch as Sink
1. Will use Landoop's fast-data-dev docker image to setup Kafka and Kafka Connect.
2. Copy the docker-compose.yml file from [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/docker-compose.yml) to a local folder.
3. Run the below command to start the kafka-cluster(along with kafka connector), postgres and elastic search.

         docker-compose up kafka-cluster postgres elasticsearch dejavu

** Note:  
* Make sure docker is installed on the machine.                                                   
* Kafka Broker will be available at [localhost:9092](localhost:9092).
* Landoop provides a web interface to manage Kafka Connect at [localhost:3030](localhost:3030).
* Postgres will be available at [localhost:5432](localhost:5432).
* Elastic Search will be available at [localhost:9200](localhost:9200). Validate the same by hitting the URL in the browser.
* Atleast 4GB of memory is required to run the fast-data-dev container.
* Dejavu is a web UI for Elasticsearch. It will be available at [localhost:1358](localhost:1358).
          
4. Go to Landoop's web interface and click on the `Connectors` tab. Click on `Elastic Search` under `Sinks` and add the below configuration.

       name=sink-elastic-distributed
       connector.class=io.confluent.connect.elasticsearch.ElasticsearchSinkConnector
       type.name=kafka-connect
       key.converter.schemas.enable=true
       tasks.max=2
       topics=postgresemployee
       value.converter.schemas.enable=true
       value.converter=org.apache.kafka.connect.json.JsonConverter
       connection.url=http://elasticsearch:9200
       key.ignore=true
       key.converter=org.apache.kafka.connect.json.JsonConverter
       topic.schema.ignore=true

5. Insert data into the postgres table `employee` as explained [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/Database/DB_Connectors_Distributed.md) and check the data in the Elastic Search index. Verify the data from 
   Dejavu's web interface or 

      http://127.0.0.1:1358/?appname=*&url=http%3A%2F%2F127.0.0.1%3A9200&mode=edit

   Using the below command.

       curl -X GET "localhost:9200/postgresemployee/_search?pretty"