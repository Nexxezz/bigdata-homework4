# bigdata-homework4
TASK 2.
1.Load weather data (.parquet) from HDFS into Hive table. 
 - download dataset (see courses.epam.com, 'hdfs homework' section)
 - download jar to lib  
 wget https://repo.hortonworks.com/content/repositories/releases/org/apache/hive/kafka-handler/3.1.0.3.1.0.6-1/kafka-handler-3.1.0.3.1.0.6-1.jar
 - docker cp lib sandbox-hdp:/usr/hdp/3.0.1.0-187/hive/
 - put dataset for homework into hdp via docker cp /local/path/ sandbox-hdp:/home/
 - unzip all .zip
 - put dataset into hdfs via hdfs dfs -put /path/to/dataset /201bd/dataset/
 - hdfs dfs -chmod -R 777 /201bd

2. Load data into Kafka using Hive-Kafka integration (Hive v3.x is required) or Kafka Connect API (jdbc connector).   
 - add jar to hive:  
   ADD JAR /root/201bd/jars/kafka-handler-3.1.0.3.1.0.6-1.jar
 - create hive table for weather data:  
 CREATE EXTERNAL TABLE weather /
  (lng double,lat double,avg_tmpr_f double,avg_tmpr_c double,wthr_date string) / 
  STORED AS PARQUET LOCATION 'hdfs://sandbox-hdp.hortonworks.com:8020/tmp/dataset/weather/';

 - create table for hive-kafka integration:  
 CREATE EXTERNAL TABLE weather_kafka (lng double,lat double,avg_tmpr_f double,avg_tmpr_c double,wthr_date string) /
  STORED BY 'org.apache.hadoop.hive.kafka.KafkaStorageHandler' /
  TBLPROPERTIES ("kafka.topic" = "weather-topic", "kafka.bootstrap.servers" = "sandbox-hdp.hortonworks.com:6667");  
  - put data into kafka topic via kafka-hive integration:  
  INSERT INTO TABLE weather_kafka  
  INSERT INTO TABLE weather_kafka SELECT `lng`, `lat`, `avg_tmpr_f`,`avg_tmpr_c`, `wthr_date`, null as `__key`, null as `__partition`, -1 as `__offset`, null as `__timestamp` from weather;
