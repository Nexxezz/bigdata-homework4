# bigdata-homework4
TASK 2.
1.Load weather data (.parquet) from HDFS into Hive table. 
  a. put dataset for homework into hdp via docker cp /local/path/ sandbox-hdp:/home/
  b. put dataset into hdfs via hdfs dfs -put /path/to/dataset /home/
2. Load data into Kafka using Hive-Kafka integration (Hive v3.x is required) or Kafka Connect API (jdbc connector).
  a. create hive table for weather data:
  CREATE EXTERNAL TABLE weather /
  (lng double,lat double,avg_tmpr_f double,avg_tmpr_c double,wthr_date string) / 
  STORED AS PARQUET LOCATION 'hdfs://sandbox-hdp.hortonworks.com:8020/tmp/weather/';
  b. create table for hive-kafka integration:
  CREATE EXTERAL TABLE weather_kafka (lng double,lat double,avg_tmpr_f double,avg_tmpr_c double,wthr_date string) /
  STORED BY 'org.apache.hadoop.hive.kafka.KafkaStorageHandler' /
  TBLPROPERTIES ("kafka.topic" = "weather-topic", "kafka.bootstrap.servers" = "sandbox-hdp.hortonworks.com:6667");
  c. put data into kafka topic via kafka-hive integration:
  INSERT INTO TABLE weather_kafka 
  SELECT `lng`, `lat`, `avg_tmpr_f`,`avg_tmpr_c`, `wthr_date`, null as `__key`, null as `__partition`, -1 as `__offset`, null as
  `__timestamp` from weather;
