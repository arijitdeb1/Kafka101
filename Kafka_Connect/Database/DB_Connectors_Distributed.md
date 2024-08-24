# Kafka Connect configurations to set Database as source/sink

## To set Database(Postgres) as Source
1. Will use Landoop's fast-data-dev docker image to setup Kafka and Kafka Connect.
2. Copy the docker-compose.yml file from [here](https://github.com/arijitdeb1/Kafka101/blob/main/Kafka_Connect/docker-compose.yml) to a local folder.
3. Run the below command to start the kafka-cluster(along with kafka connector), postgres and elastic search.
        
         docker-compose up kafka-cluster postgres elasticsearch dejavu

** Note:  1. Make sure docker is installed on the machine.                                                   
          2. Kafka Broker will be available at [localhost:9092](localhost:9092).
          3. Landoop provides a web interface to manage Kafka Connect at [localhost:3030](localhost:3030).
          4. Postgres will be available at [localhost:5432](localhost:5432).
          5. Postgres container will be created with the below configuration.          
                
    POSTGRES_USER: postgres
    POSTGRES_PASSWORD: postgres
    POSTGRES_DB: postgres

4. Start a shell in the postgres container and create a table and insert some data.

       docker exec -it <postgres-container_id> psql -U postgres

       \c - Connect to the database
       \dt - List all the tables

5. Create a table and insert some data.

       CREATE TABLE employee (
       employee_id SERIAL PRIMARY KEY,
       employee_sso VARCHAR(255) UNIQUE NOT NULL,
       employee_name VARCHAR(255) NOT NULL,
       cio_org VARCHAR(255),
       ee_start_date DATE,
       ee_end_date DATE,
       billing_hub_id BIGINT,
       supervisor VARCHAR(255),
       band VARCHAR(50),
       status VARCHAR(50)
       );
          
6. Go to Landoop's web interface and click on the `Connectors` tab. Click on `Jdbc` under `Sources` and add the below configuration.

       name=source-postgres-distributed
       connector.class=io.confluent.connect.jdbc.JdbcSourceConnector
       incrementing.column.name=employee_id
       connection.password=postgres
       tasks.max=1
       connection.attempts=3
       plugin.path=/opt/lensesio/connectors/third-party/kafka-connect-jdbc
       connection.backoff.ms=10000
       table.whitelist=employee
       mode=incrementing
       topic.prefix=postgres
       connection.user=postgres
       value.converter=org.apache.kafka.connect.json.JsonConverter
       connection.url=jdbc:postgresql://postgres:5432/postgres
       insert.mode=upsert
       transforms=route
       transforms.route.type=org.apache.kafka.connect.transforms.RegexRouter
       transforms.route.regex=(.+)
       transforms.route.replacement=postgresemployee

** Note: 1. Wait a few seconds for the connector to start. You can check the status of the connector in the Connectors tab.
         2. For any issues, start a bash shell in the kafka-cluster container and check the logs of the connector - [/var/log/connect-distributed.log](/)
                  
           docker exec -it <kafka-cluster-container_id> bash

7. Insert data into the employee table 
    
       INSERT INTO employee (employee_sso, employee_name, cio_org, ee_start_date, ee_end_date, billing_hub_id, supervisor, band, status)
       VALUES
        ('SSO112233', 'Michael Johnson', 'Engineering', '2023-05-01', NULL, 1, 'Sarah Lee', 'Engineer', 'Active'),
        ('SSO445566', 'Linda Martinez', 'Sales', '2023-06-15', '2024-06-14', 2, 'Chris Walker', 'Senior Sales', 'Active'),
        ('SSO778899', 'James Brown', 'Marketing', '2022-12-01', NULL, 3, 'Patricia Taylor', 'Marketing Specialist', 'Inactive'),
        ('SSO223344', 'Jessica Wilson', 'HR', '2024-01-20', NULL, 4, 'Daniel Miller', 'HR Manager', 'Probationary'),
        ('SSO556677', 'David Anderson', 'Finance', '2023-08-01', NULL, 5, 'Jessica Lewis', 'Accountant', 'Active'),
        ('SSO889900', 'Emily Clark', 'IT', '2022-10-01', '2023-10-01', 6, 'William Scott', 'IT Support', 'Terminated'),
        ('SSO667788', 'Ryan White', 'Operations', '2023-04-01', NULL, 7, 'Nancy Young', 'Operations Lead', 'Active'),
        ('SSO334455', 'Sophie Adams', 'Product', '2023-07-01', '2024-07-01', 8, 'Charles King', 'Product Manager', 'Active'),
        ('SSO998877', 'Olivia Turner', 'Customer Service', '2024-03-01', NULL, 9, 'Lucas Wright', 'Customer Support Specialist', 'Active'),
        ('SSO556688', 'Ethan Hall', 'R&D', '2022-11-15', '2023-11-15', 10, 'Megan Robinson', 'Research Scientist', 'Inactive');


    
12. Check the data in the topic **`postgresemployee`** which gets automatically created by the kafka connect. Verify the data from Landoop's web interface or using the below command.

        kafka-console-consumer --topic postgres-employee --from-beginning --bootstrap-server localhost:9092


