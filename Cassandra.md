## Run YCSB in Cassandra cluster

Create Cassandra cluster using docker. First you need to update your system:

```sh
sudo apt-get update
```

We can now install Docker; make sure you type Y for Yes when prompted.

```sh
sudo apt-get install docker.io
```

Next step is to create a create a Cassandra container.

```sh
docker run --name my-cassandra-1 -d cassandra:3.11
```

Let's use the nodetool command in cassandra-1 to check if our Cassandra node is up and running.

```sh
docker exec -i -t cassandra-1 bash -c 'nodetool status
```

Let's connect our second container. Run the following command to create the second Cassandra node cassandra-2.

```sh
docker run --name cassandra-2 -d --link cassandra-1:cassandra cassandra:3.11
```

Similar for a third node

```sh
docker run --name cassandra-3 -d --link cassandra-1:cassandra cassandra:3.11
```

Inspect your cluster again:

```sh
docker exec -i -t cassandra-1 bash -c 'nodetool status
```

### Creating a table for use with YCSB

For keyspace ycsb, table usertable:

**Hint: to access docker container cqlsh enter the following command (if Cassandra cluster deployed in docker)

```sh
docker exec -it cassandra-1 bash -c 'cqlsh'
```

Then:

```sh
cqlsh> create keyspace ycsb WITH REPLICATION = {'class' : 'SimpleStrategy', 'replication_factor': 3 };
cqlsh> USE ycsb;
cqlsh> create table usertable (
    y_id varchar primary key,
    field0 varchar,
    field1 varchar,
    field2 varchar,
    field3 varchar,
    field4 varchar,
    field5 varchar,
    field6 varchar,
    field7 varchar,
    field8 varchar,
    field9 varchar);
```

Finally load/run the workload:

```sh
./bin/ycsb load cassandra-cql -s -P workloads/workloada -p "hosts=172.17.0.2"
```

```sh
./bin/ycsb run cassandra-cql -s -P workloads/workloada -p "hosts=172.17.0.2" -p status.interval=1 > outputRun.txt

```
