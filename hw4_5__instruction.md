# Instructions for homework №4-5. Apache spark + yarn

### To execute this instruction, the steps in instruction hw_1-3 must be completed

*First, put data in '/input' folder*

**Next, our goal is to store data and access it optimally. In addition, we want to do all this automatically. Our actions:**
1) Start Spark
2) Partition the table (to make it easier and faster to access it in the future just by dividing it by these partitions)
3) Save the partitioned table
4) Automate this process

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
from onetl.db import DBWriter
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

# checks
df.rdd.fetNumPartitions()
df.write.parquet('Sleep_health_and_lifestyle_dataset.csv')

df = df.repartition(3, 'dt_month')

hive = Hive(spark=spark, cluster='test')
writer = DBWriter(connection=hive, table='test.health_by_months', options={'if_exists':'replace_entire_table', 'partitionBy': 'dt_month'})
writer.run(df)
df.write.saveAsTable('health_by_months', partitionBy='dt_month')

spark.stop()
```

## Part 2: Automatic repartition
```bash
vim perfect_flow.py
```

**put this to the file:**

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F 

from onetl.connection import SparkHDFS
from onetl.db import DBWriter
from onetl.file import FileDFReader
from onetl.file.format import CSV

from prefect import flow, task


@task
def create_session():
  spark = SparkSession.builder \
                      .master("yarn") \
                      .appName("spark-with-yarn") \
                      .config("spark.sql.warehouse.dir", "/user/hive/warehouse") \
                      .config("spark.hive.metastore.uris", "thrift://tmpl-dn-01:9083") \
                      .enableHiveSupport() \
                      .getOrCreate()
  return spark

@task
def extract_data(spark):
  hdfs = SparkHDFS(host='tmpl-nn', port=9000, spark=spark, cluster='test')
  hdfs.check()
  
  reader = FileDFReader(connection=hdfs, format=CSV(delimeter=',', header=True), source_path='/input')
  df = reader.run(['Sleep_health_and_lifestyle_dataset.csv'])

  return df

@task
def transform_data(_df):
  df = _df.withColumn('dt_month', F.col('dt').substr(0,6))
  df = df.repartition(3, 'dt_month')

  return df

@task
def load_data(_df, spark):
  hive = Hive(spark=spark, cluster='test')
  writer = DBWriter(connection=hive, table='test.health_by_months', options={'if_exists':'replace_entire_table', 'partitionBy': 'dt_month'})
  writer.run(_df)

@flow
def process_data():
  spark_session = create_session()
  edata = extract_data(spark_session)
  tdata = transform_data(edata)
  load_data(tdata, spark_session)


if __name__ == '__main__':
  process_data()
```
