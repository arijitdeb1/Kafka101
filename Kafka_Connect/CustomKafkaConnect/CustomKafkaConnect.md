## How to create your own kafka connector 

1. will create a source connector to read data from GitHub API[GET /repos/:owner/:repo/issues] and push it to a Kafka topic.
   https://api.github.com/repos/kubernetes/kubernetes/issues



psql -U postgres -d mydb

SELECT * FROM pg_replication_slots WHERE slot_name = 'debezium';
SELECT * FROM pg_publication;

CREATE TABLE public.my_table (
id SERIAL PRIMARY KEY,
name VARCHAR(100) NOT NULL,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO public.my_table (name) VALUES ('Arijit');