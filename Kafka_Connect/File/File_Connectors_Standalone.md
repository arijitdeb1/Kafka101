# Kafka Connect configurations to set File as source/sink

## To set File as Source
1. Will use Landoop's fast-data-dev docker image to setup Kafka and Kafka Connect.
2. Copy the docker-compose.yml file from [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/docker-compose.yml) to a local folder.
3. Run the below command to start the fast-data-dev docker container.

        docker-compose up kafka-cluster
   
   ** Note:  1. Make sure docker is installed on the machine.                                                   
             2. Kafka Broker will be available at localhost:9092.
             3. Landoop provides a web interface to manage Kafka Connect at localhost:3030.

4. Create a file source connector - [file-stream-demo-standalone.properties](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/File/file-stream-demo-standalone.properties) . It provides the configuration to set the file as a source.
5. Create the worker.properties file - [worker.properties](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/File/worker.properties) . It provides the details of kafka broker and key/value converters.
6. Create a file as mentioned in the connector configuration  - **demo-file.txt**
7. Spin up another instance of the fast-data-dev container to create the required topic and also to mount the above files to a container volume.

        docker run --rm -it -v %cd%:/tutorial --net=host landoop/fast-data-dev:cp3.3.0 bash

8. Above command will start a bash shell in the container. Run the below command to create a topic.

        kafka-topics --create --topic demo-1-standalone --partitions 3 --replication-factor 1 --zookeeper 127.0.0.1:2181

9. Go the mounted folder and run the below command to start the connector.Maintain teh order of the files in the command.

        connect-standalone worker.properties file-stream-demo-standalone.properties

10. Add some data to the file **demo-file.txt** and check the data in the topic **demo-1-standalone**. Verify the data from Landoop's web interface or using the below command.s

        kafka-console-consumer --topic demo-1-standalone --from-beginning --bootstrap-server

       
             

        
