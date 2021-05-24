#Create a stream with JSON

## Create a `USERPROFILE` topic
`docker-compose exec broker kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic USERPROFILE`

## Using a blob of json to describe the data within the topic
`docker container exec -i broker kafka-console-producer --broker-list localhost:9092 --topic USERPROFILE << EOF` 

`{"userid":1000, "firstname": "Fermin", "lastname":"Fernandez", "countrycode": "VE", "rating":4.7}`
`{"userid":1001, "firstname": "Adrian", "lastname":"Fernandez", "countrycode": "ES", "rating":4.7}`

`EOF`

## Connect to KSQL
`docker-compose exec ksqldb-cli ksql http://ksqldb-server:8088`

## Create a stream
`CREATE STREAM userprofile (userid INT, firstname VARCHAR, lastname VARCHAR, countrycode VARCHAR, rating DOUBLE) WITH (VALUE_FORMAT = 'JSON', KAFKA_TOPIC = 'USERPROFILE');`

## Check creation of stream
`list streams;`

## Check the stream structure using `describe [stream]`
`describe userprofile;`

```bash
Name                 : USERPROFILE
 Field       | Type            
-------------------------------
 USERID      | INTEGER         
 FIRSTNAME   | VARCHAR(STRING) 
 LASTNAME    | VARCHAR(STRING) 
 COUNTRYCODE | VARCHAR(STRING) 
 RATING      | DOUBLE          
-------------------------------
```

## Create a simple query
`select userid, firstname, lastname, countrycode, rating from userprofile emit changes;`

## Drop a stream
`drop stream if exists userprofile delete topic;`