#Generating demo data using `ksql-datagen`
* Command line tool
* Generate random 
* Use provided custom schema

## Loading `avro` file with schema

`docker-compose exec ksql-datagen ksql-datagen bootstrap-server=broker:29092 schema=/tmp/userprofile.avro format=json topic=USERPROFILE msgRate=1 iterations=100 key=userid`

## Connect to KSQL
`docker-compose exec ksqldb-cli ksql http://ksqldb-server:8088`

## Create a `USERPROFILE` topic
`docker-compose exec broker kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic USERPROFILE`

## create a stream
`
create stream USERPROFILE(userid INT, firstname VARCHAR, lastname VARCHAR, countrycode VARCHAR, rating DOUBLE) with (KAFKA_TOPIC='USERPROFILE', VALUE_FORMAT='json');
`

## Pick some data each an amount of time
`print 'USERPROFILE' interval 5;`

## 1. Shows producer time and username 
`select rowtime, firstname from USERPROFILE emit changes;`
- `rowtime` field shows time using EPOC format.
- `rowkey` represents the key of the topic


## 2. Human-readable date using a scalar function `TIMESTAMPTOSTRING(TIME, FORMAT)`
`select timestamptostring(rowtime, 'dd/MMM HH:mm') as createtime, firstname from userprofile emit changes;`

## 3. Concat values 
`select timestamptostring(rowtime, 'dd/MMM HH:mm') as createtime, concat(firstname,' ',ucase(lastname)) as full_name from userprofile emit changes;`


### Scalar functions
https://docs.ksqldb.io/en/latest/developer-guide/ksqldb-reference/scalar-functions/

