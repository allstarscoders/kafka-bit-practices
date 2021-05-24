# KSQL from Confluent

- Source available project at https://www.confluent.io/ksql
- Writing kafka streams java applications is complex
- Sometimes, you just want to write... SQL!

```SQL
CREATE STREAM vip_actions AS 
SELECT userid, page, action
FROM clickstream c
LEFT JOIN users u ON c.userid = u.user_id 
WHERE u.level = 'Platinum';
```
- Underneath, Kafka Streams applications are generated
- We get the same benefits(scale, security)

## Start with
`docker-compose up -d`

## Examples

- [STAGE ONE](STAGEONE.md), simple query grouping elements.
- [STAGE TWO](STAGETWO.md), streams creation using Json format and simple queries.
- [STAGE THREE](STAGETHREE.md), generate demo data and scalar functions is introduced.

