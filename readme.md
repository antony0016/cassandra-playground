# Cassandra playground

## Aim of the project

The aim of this project is follow the official documentation of cassandra to learn how to use it.
[offical tutorial link](https://cassandra.apache.org/_/quickstart.html)

## Step of the project

1. Run a cassandra instance
Use docker to run a cassandra instance and network.

2. Create data.cql
Create a file to create a keyspace and a table.

3. Use cqlsh
Use the cqlsh to run data.cql and interact with the cassandra instance.

4. Clean up
Remove the cassandra instance and the network.

## Step 1: Run a cassandra instance

```bash
# Create a network to make the cqlsh able to connect to the cassandra instance.
docker network create cassandra

docker run --rm -d \
--name cassandra \
--hostname cassandra \
--network cassandra cassandra
```

## Step 2: Create data.cql

```sql
-- Create a keyspace
CREATE KEYSPACE IF NOT EXISTS store WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : '1' };

-- Create a table
CREATE TABLE IF NOT EXISTS store.shopping_cart (
userid text PRIMARY KEY,
item_count int,
last_update_timestamp timestamp
);

-- Insert some data
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('9876', 2, toTimeStamp(now()));
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('1234', 5, toTimeStamp(now()));
```

## Step 3: Use cqlsh

- Use cqlsh to execute data.cql

```bash
docker run --rm --network cassandra \
-v "$(pwd)/data.cql:/scripts/data.cql" \
-e CQLSH_HOST=cassandra -e CQLSH_PORT=9042 \
-e CQLVERSION=3.4.6 \
nuvo/docker-cqlsh
```

- Interact with the cassandra instance

```bash
docker run --rm -it \
--network cassandra \
nuvo/docker-cqlsh cqlsh cassandra 9042 --cqlversion='3.4.5'
```

## Step 4: Clean up

Run the following commands to clean up the cassandra instance and the network.

```bash
docker kill cassandra

docker network rm cassandra
```
