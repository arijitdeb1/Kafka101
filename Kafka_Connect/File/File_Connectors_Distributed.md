# Kafka Connect configurations to set File as source/sink

## To set File as Source
1. Will use Landoop's fast-data-dev docker image to setup Kafka and Kafka Connect.
2. Copy the docker-compose.yml file from [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/docker-compose.yml) to a local folder.
3. Run the below command to start the fast-data-dev docker container.

        docker-compose up kafka-cluster
   
   ** Note:  1. Make sure docker is installed on the machine.                                                   
             2. Kafka Broker will be available at localhost:9092.
             3. Landoop provides a web interface to manage Kafka Connect at localhost:3030.

4. In Distributed mode, we will provide the connector configuration using the Landoop's web interface.
5. Go to Landoop's web interface and click on the Connectors tab. Click on `File` under `Sources` and add the below configuration.

                name=file-stream-demo-distributed
                connector.class=org.apache.kafka.connect.file.FileStreamSourceConnector
                tasks.max=1
                file=demo-file.txt
                topic=demo-2-distributed
                key.converter=org.apache.kafka.connect.json.JsonConverter
                key.converter.schemas.enable=true
                value.converter=org.apache.kafka.connect.json.JsonConverter
                value.converter.schemas.enable=true

6. Spin up another instance of the fast-data-dev container to create the required topic and also to mount the above files to a container volume.
        
        docker run --rm -it --net=host landoop/fast-data-dev:cp3.3.0 bash 
        kafka-topics --create --topic demo-2-distributed --partitions 3 --replication-factor 1 --zookeeper 127.0.0.1:2181

7. No worker.properties file is required in distributed mode as kafka connect will take care of spinning up the workers.
8. Start a bash shell in the container created at Step 3 and create the file mentioned.
   
       docker exec -it <container_id> bash


9. Add some data to the file **demo-file.txt** and check the data in the topic **demo-1-standalone**. Verify the data from Landoop's web interface or using the below command.s

        kafka-console-consumer --topic demo-2-distributed --from-beginning --bootstrap-server


       
             

        
