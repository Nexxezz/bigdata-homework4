# bigdata-homework4
TASK 2.
1. Put dataset for homework into hdp via docker cp /local/path/ sandbox-hdp:/home/
2. Put dataset into hdfs via hdfs dfs -put /path/to/dataset /home/
3. Create hive table for weather data:
CREATE EXTERNAL TABLE weather /
(lng double,lat double,avg_tmpr_f double,avg_tmpr_c double,wthr_date string) /
STORED AS PARQUET LOCATION 'hdfs://sandbox-hdp.hortonworks.com:8020/tmp/weather/';
4. Create table for hive-kafka connector:
CREATE EXTERAL TABLE weather_kafka (lng double,lat double,avg_tmpr_f double,avg_tmpr_c double,wthr_date string) /
STORED BY 'org.apache.hadoop.hive.kafka.KafkaStorageHandler' /
TBLPROPERTIES ("kafka.topic" = "weather-topic", "kafka.bootstrap.servers" = "sandbox-hdp.hortonworks.com:6667");
5. Put data into kafka topic via hive connector:
INSERT INTO TABLE weather_kafka 
SELECT `lng`, `lat`, `avg_tmpr_f`,`avg_tmpr_c`, `wthr_date`, null as `__key`, null as `__partition`, -1 as `__offset`, null as `__timestamp` from weather;
