# Basics in command line

## Launch all services
`docker-compose up -d`

## Connect to KSQL
`docker-compose exec ksqldb-cli ksql http://ksqldb-server:8088`

## Common commands
* `list topics;` - Shows all topics available
* `print [topic];` - Shows the topic data
* `list streams;` - Shows all stream available
* `show streams;` - Shows all stream available

## Create USERS topic
`
docker-compose exec broker kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic USERS
`

## Produce data
`
docker container exec -it broker kafka-console-producer --broker-list localhost:9092 --topic USERS
`

# Got it!. Let's create streams

## Create a stream based in an existent topic
`
create stream users_stream(name VARCHAR, countrycode VARCHAR) WITH (KAFKA_TOPIC='USERS', VALUE_FORMAT='DELIMITED');
`
## We can verify creation of stream using `list streams;` command

## We can show messages from the beginning of time using `auto.offset.reset` configuration
`SET 'auto.offset.reset' = 'earliest';`

## Querying stream 
`select name, countrycode from users_stream emit changes;`

## Also, we can show a number of records using `limit n` command
`select name, countrycode from users_stream emit changes limit 4;`

# Aggregations

## Grouping values using `group by`
`select countrycode, count(*) from users_stream group by countrycode emit changes;`

## Drop a stream
`drop stream if exists USERS_STREAM delete topic;`

