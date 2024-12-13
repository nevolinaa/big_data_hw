# Instructions for homework №4-5. Apache spark + yarn

## Part 1: Apache spark and repartition

```bash
sudo -i -u hadoop
source venv/bin/activate
ipython
```

**switch to the interactive python editor**
```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F 
from onetl.connection import SparkHDFS
from onetl.file import FileDFReader
from onetl.file.format import CSV


spark = SparkSession.builder \
                    .master("yarn") \
                    .appName("spark-with-yarn") \
                    .config("spark.sql.warehouse.dir", "/user/hive/warehouse") \
                    .config("spark.hive.metastore.uris", "thrift://tmpl-dn-01:9083") \
                    .enableHiveSupport() \
                    .getOrCreate()

hdfs = SparkHDFS(host='tmpl-nn', port=9000, spark=spark, cluster='test')

# To check that everything is correct
hdfs.check()

reader = FileDFReader(connection=hdfs, format=CSV(delimeter=',', header=True), source_path='/input')

df = reader.run(['Sleep_health_and_lifestyle_dataset.csv'])

# To check that everything is correct
print(df.count())
df.printSchema()

dt = df.select('Date')
# To check dates
dt.show()

df = df.withColumn('dt_month', F.col('dt').substr(0,6))
df.rdd.fetNumPartitions()

df.write.parquet('Sleep_health_and_lifestyle_dataset.csv')

df = df.repartition(3, 'dt_month')

hive = Hive(spark=spark, cluster='test')
writer = DBWriter(connection=hive, table='test.health_by_months', options={'if_exists':'replace_entire_table', 'partitionBy': 'dt_month'})
writer.run(df)
df.write.saveAsTable('health_by_months', partitionBy='dt_month')
```

## Part 2: Airflow
