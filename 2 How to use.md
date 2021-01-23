# Run Carbon on your computer

## 1. Get Carbondata Release

You can get released Carbondata jar from [Github](https://github.com/apache/carbondata/releases) or [Apache achieve](https://dist.apache.org/repos/dist/release/carbondata/). As for each module, you can find it in [maven repository](https://search.maven.org/search?q=g:org.apache.carbondata)

## 2. Prepare environment

Recommand:
- Linux OS
- JDK 7
- Hadoop 2.7.2
- Spark 2.3

Please ensure that HDFS, MapReduce and Spark is working normally. You can check it by running example job, such as `Word Count` for MapReduce and `PI calculation` for Spark.

Also, you need to prepare `carbon.properties` file as `https://github.com/apache/carbondata/tree/master/conf/carbon.properties.template`

You can config `carbon.properties.filepath` in spark's  `spark-defaults.conf` file, or set `extraJavaOptions` when submit application.
Please ensure `carbon.properties` file can be distributed to each executor, else parameters will not take effect.


## 3. Run Carbon
We can submit the assembly jar file as a spark application to create `CarbonThriftServer`, and then use `beeline` to connect.

> Format
```
./bin/spark-submit \
  --class org.apache.carbondata.spark.thriftserver.CarbonThriftServer  \
  --master <master-url> \
  --deploy-mode <deploy-mode> \
  --conf <key>=<value> \
  ... # other options
  <CARBON_ASSEMBLY_JAR_PATH> \
  <CARBON_STORE_PATH>
```

> Example
```
$SPARK_HOME/bin/spark-submit \
  --class org.apache.carbondata.spark.thriftserver.CarbonThriftServer  \
  --master yarn \
  --deploy-mode client \
  --driver-memory 10g \
  --num-executors 3 \
  --executor-memory 10g \ 
  --executor-cores 10 \
  --conf -Dcarbon.properties.filepath = carbon.properties \
  apache-carbondata-1.5.1-bin-spark2.2.1-hadoop2.7.2.jar \
  hdfs://localhost:9000/user/hive/warehouse/carbon.store
```

When you see a log saying that Thrift Sever is listening to 127.0.0.1:10000, you can use following command to connect:
```
$SPARK_HOME/bin/beeline -u jdbc:hive2://localhost:10000 -n root
```

## Use Carbon to manage your data

Simple usage example

```
// create table
CREATE TABLE IF NOT EXISTS carbon_test_table (col_1 string , col_2 int)
STORED AS carbondata;

// show table 
SHOW TABLES;

// insert data
INSERT INTO TABLE carbon_test_table VALUES('a',1),('b',2);

// show segments
SHOW SEGMENTS FOR TABLE carbon_test_table;

// query
SELECT * FROM carbon_test_table WHERE col_2=1;
```

And you can check data file in <CARBON_STORE_PATH> which is set when you start CarbonThriftServer
