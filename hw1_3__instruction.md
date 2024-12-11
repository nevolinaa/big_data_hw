# Instructions for homework №1-3

```bash
git clone git@github.com:nevolinaa/big_data_hw.git
cd big_data_hw
ssh -o "IdentitiesOnly=yes" -i ./ssh/id_rsa team@176.109.91.17
```
**wherever we don't specify what information to enter, just press enter or ‘yes’**

## Part 1: hadoop
```bash
sudo adduser hadoop
```
*password: 1234567890*  
*full name: hadoop*

```bash
sudo -i -u hadoop

ssh-keygen 
cat .ssh/id_ed25519.pub
```

*copy key include ssh-ed25519 and exclude hadoop@team-15-jn*

```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz

exit

sudo vim /etc/hosts
```

**comment all and put this to the file:**  
*192.168.1.62 team-15-jn*  
*192.168.1.63 team-15-nn*  
*192.168.1.64 team-15-dn-0*  
*192.168.1.65 team-15-dn-1*  

```bash
ssh team-15-nn
```

**repeat:**  
```bash
sudo adduser hadoop
```
*password: 1234567890*  
*full name: hadoop*  

```bash
sudo -i -u hadoop

ssh-keygen
cat .ssh/id_ed25519.pub
```

*copy key include ssh-ed25519 and exclude hadoop@team-15-nn*  

```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz

exit

sudo vim /etc/hosts
```

**comment all and put this to the file:**  
*192.168.1.62 team-15-jn*  
*192.168.1.63 team-15-nn*  
*192.168.1.64 team-15-dn-0*  
*192.168.1.65 team-15-dn-1*  

```bash
exit

ssh team-15-dn-0
```

**repeat:**
```bash
sudo adduser hadoop
```
*password: 1234567890*  
*full name: hadoop*  

```bash
sudo -i -u hadoop

ssh-keygen
cat .ssh/id_ed25519.pub
```

*copy key include ssh-ed25519 and exclude hadoop@team-15-dn-00*

```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz

exit

sudo vim /etc/hosts
```

**comment all and put this to the file:**  
*192.168.1.62 team-15-jn*  
*192.168.1.63 team-15-nn*  
*192.168.1.64 team-15-dn-0*  
*192.168.1.65 team-15-dn-1*  

```bash
exit

ssh team-15-dn-1
```

**repeat:** 
```bash
sudo adduser hadoop
```
*password: 1234567890*  
*full name: hadoop*  

```bash
sudo -i -u hadoop

ssh-keygen
cat .ssh/id_ed25519.pub
```

*copy key include ssh-ed25519 and exclude hadoop@team-15-dn-01*

```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz

exit
sudo vim /etc/hosts
```

**comment all and put this to the file:**  
*192.168.1.62 team-15-jn*  
*192.168.1.63 team-15-nn*  
*192.168.1.64 team-15-dn-0*  
*192.168.1.65 team-15-dn-1*  

```bash
exit

sudo -i -u hadoop 
vim .ssh/authorized_keys
```

**put your keys to this file. you should have smth like:**  
*ssh-ed25519 key-jn*  
*ssh-ed25519 key-nn*  
*ssh-ed25519 key-dn-0*  
*ssh-ed25519 key-dn-1*  

```bash
exit

ssh team-15-nn 
sudo -i -u hadoop 
vim .ssh/authorized_keys
```

**put your keys to this file. you should have smth like:**  
*ssh-ed25519 key-jn*  
*ssh-ed25519 key-nn*  
*ssh-ed25519 key-dn-0*  
*ssh-ed25519 key-dn-1*  

```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz
tar -xvzf hadoop-3.4.0.tar.gz
exit
```
(you should exit from node, not hadoop user. if needed, do sudo -i -u hadoop again)

```bash
ssh team-15-dn-0
vim .ssh/authorized_keys
```

**put your keys to this file. you should have smth like:**  
*ssh-ed25519 key-jn*  
*ssh-ed25519 key-nn*  
*ssh-ed25519 key-dn-0*  
*ssh-ed25519 key-dn-1*  

```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz
tar -xvzf hadoop-3.4.0.tar.gz
exit
```
(you should exit from node, not hadoop user. if needed, do sudo -i -u hadoop again)

```bash
ssh team-15-dn-1
vim /home/hadoop/.ssh/authorized_keys
```

**put your keys to this file. you should have smth like:**  
*ssh-ed25519 key-jn*  
*ssh-ed25519 key-nn*  
*ssh-ed25519 key-dn-0*  
*ssh-ed25519 key-dn-1*  

```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz
tar -xvzf hadoop-3.4.0.tar.gz
exit
```
(you should exit from node, not hadoop user. if needed, do sudo -i -u hadoop again)

```bash
ssh team-15-nn

vim ~/.profile
```

**put in the end of the file:**  
*export HADOOP_HOME=/home/hadoop/hadoop-3.4.0*  
*export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64*  
*export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin*  

```bash
source ~/.profile

cd hadoop-3.4.0/etc/hadoop
vim hadoop-env.sh
```

## Part 2: change configs

**put this line to the file between**  
*‘# JAVA_HOME=/usr/java/testing hdfs dfs -ls*  
*and*  
*‘#*  
*# Therefore, the vast majority (BUT NOT ALL!) of these defaults’*  
*lines:*  
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

```bash
vim core-site.xml 
```

**put these lines to the file into configuration block:**  
`<configuration>`  
`    <property>`  
`        <name>fs.defaultFS</name>`  
`        <value>hdfs://team-15-nn:9000</value>`  
`    </property>`  
`</configuration>`  

```bash
vim hdfs-site.xml
```

**put these lines to the file into configuration block:**  
`<configuration>`  
`    <property>`  
`        <name>dfs.replication</name>`  
`        <value>3</value>`  
`    </property>`  
`</configuration>`  

```bash
vim workers
```

**delete localhost from the file**  
**and put these lines to the file:**  
*team-15-nn*  
*team-15-dn-0*  
*team-15-dn-1*  
```bash
exit

ssh team-15-dn-0
wget -O .profile https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/.profile
cd hadoop-3.4.0/etc/hadoop/
wget -O hadoop-env.sh https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/hadoop-env.sh
wget -O core-site.xml https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/core-site.xml
wget -O hdfs-site.xml https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/hdfs-site.xml
wget -O workers https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/workers

exit
```
(you should exit from node, not hadoop user. if needed, do sudo -i -u hadoop again)

```bash
ssh team-15-dn-1
wget -O .profile https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/.profile
cd hadoop-3.4.0/etc/hadoop/
wget -O hadoop-env.sh https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/hadoop-env.sh
wget -O core-site.xml https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/core-site.xml
wget -O hdfs-site.xml https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/hdfs-site.xml
wget -O workers https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/workers

exit
```
(you should exit from node, not hadoop user. if needed, do sudo -i -u hadoop again)

```bash
ssh team-15-nn

cd hadoop-3.4.0
bin/hdfs namenode -format
sbin/start-dfs.sh
```

check: jps  
you should have DataNode, NameNode, SecondaryNameNode, Jps  

exit (from node)  

```bash
cd /etc/nginx/sites-available
sudo wget -O nn https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/nginx/sites-available/nn

sudo ln -s /etc/nginx/sites-available/nn /etc/nginx/sites-enabled/nn
sudo systemctl reload nginx
```

check:  
176.109.91.17:9870  
should be working  

```bash
ssh team-15-nn
sudo -i -u hadoop
cd hadoop-3.4.0/etc/hadoop
vim mapred-site.xml
```

**put these lines to the file into configuration block:**  
<configuration>  
   <property>  
        <name>mapreduce.framework.name</name>  
        <value>yarn</value>  
    </property>  
    <property>  
        <name>mapreduce.application.classpath</name>  
        <value>$HADOOP_HOME/share/hadoop/mapreduce/*: $HADOOP_HOME/share/hadoop/mapreduce/lib/*</value></value>  
    </property>  
</configuration>  

```bash
vim yarn-site.xml
```

**put these lines to the file into configuration block:**  
<configuration>  
    <property>  
        <name>yarn.nodemanager.aux-services</name>  
        <value>mapreduce_shuffle</value>  
    </property>  
    <property>  
        <name>yarn.nodemanager.env-whitelist</name>  
        <value> JAVA_HOME, HADOOP_COMMON_HOME, HADOOP_HDFS_HOME, HADOOP_CONF_DIR, CLASSPATH_PREPEND_DISTCACHE, HADOOP_YARN_HOME, HADOOP_HOME, PATH, LANG, TZ, HADOOP_MAPRED_HOME</value>  
    </property>  
</configuration>  

```bash
exit
exit

sudo -i -u hadoop
ssh team-15-dn-0
cd hadoop-3.4.0/etc/hadoop
wget -O mapred-site.xml https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/mapred-site.xml
wget -O yarn-site.xml https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/yarn-site.xml

exit

ssh team-15-dn-1
cd hadoop-3.4.0/etc/hadoop
wget -O mapred-site.xml
https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/mapred-site.xml
wget -O yarn-site.xml
https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/yarn-site.xml

exit
sudo -i -u hadoop
ssh team-15-nn
cd hadoop-3.4.0

sbin/start-yarn.sh
mapred --daemon start historyserver

exit 
exit
```
(from node and user)

```bash
cd /etc/nginx/sites-available
sudo wget -O ya https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/nginx/sites-available/ya
sudo wget -O dh https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/nginx/sites-available/dh

sudo ln -s /etc/nginx/sites-available/ya /etc/nginx/sites-enabled/ya
sudo ln -s /etc/nginx/sites-available/dh /etc/nginx/sites-enabled/dh

sudo systemctl reload nginx
```

check:  
176.109.91.17:8088  
176.109.91.17:19888  
should be working  

## Part 3: data

```bash
ssh team-15-nn
sudo apt install postgresql
sudo -i -u postgres
psql

CREATE DATABASE metastore;
CREATE USER hive with password 'hiveMegaPass';
GRANT ALL PRIVILEGES ON DATABASE "metastore" to hive;
ALTER DATABASE metastore OWNER to hive;
\q
exit (from user not node)

sudo vim /etc/postgresql/16/main/postgresql.conf
change line:
from #listen_addresses = 'localhost'         # what IP address(es) to listen on;
to listen_addresses = ‘team-15-nn'         # what IP address(es) to listen on;

sudo vim /etc/postgresql/16/main/pg_hba.conf
```

**put this line to the file between**  
‘# IPv4 local connections:’  
and  
‘host    all             all             127.0.0.1/32            scram-sha-256’  
lines:  
host    metastore       hive            192.168.1/62            password  

```bash
sudo systemctl restart postgresql
```

check:
```bash
sudo systemctl status postgresql
```
should be working

exit
```bash
sudo apt install postgresql-client-16
```

check:
```bash
psql -h team-15-nn -p 5342 -U hive -W -d metastore
```
should work

```bash
sudo -i -u hadoop
wget https://dlcdn.apache.org/hive/hive-4.0.0/apache-hive-4.0.0-bin.tar.gz  

tar -xvzf apache-hive-4.0.0-bin.tar.gz
cd apache-hive-4.0.0-bin/lib/
wget https://jdbc.postgresql.org/download/postgresql-42.7.4.jar
cd ../conf/ 
vim hive-site.xml
```

**put these lines to the file:**  
<configuration>  
    <property>  
        <name>hive.server2.authentication</name>  
        <value>NONE</value>  
    </property>  
    <property>  
        <name>hive.metastore.warehouse.dir</name>  
        <value>/user/hive/warehouse</value>  
    </property>  
    <property>  
        <name>hive.server2.thrift.port </name>  
        <value>5433</value>  
        <description>TCP port number to listen on, default 10000</description>  
    </property>  
    <property>  
        <name>javax.jdo.option.ConnectionURL</name>  
        <value>jdbc:postgresql://tmpl-nn:5432/metastore</value>  
    </property>  
    <property>  
        <name>javax.jdo.option.ConnectionDriverName</name>  
        <value>org.postgresql.Driver </value>  
    </property>  
    <property>  
        <name>javax.jdo.option.ConnectionUserName</name>  
        <value>hive</value>  
    </property>  
    <property>  
        <name>javax.jdo.option.ConnectionPassword</name>  
        <value>hiveMegaPass</value>  
        </property>  
</configuration>  

```bash
vim ~/.profile
```

**check if it is in the end of the file:**  
export HADOOP_HOME=/home/hadoop/hadoop-3.4.0  
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64  
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin  
export HIVE_HOME=/home/hadoop/apache-hive-4.0.0-bin  
export HIVE_CONF_DIR=$HIVE_HOME/conf  
export HIVE_AUX_JARS_PATH=$HIVE_HOME/lib/*  
export PATH=$PATH:$HIVE_HOME/bin  

```bash
source ~/.profile
hive --version

hdfs dfs -mkdir -p /user/hive/warehouse
hdfs dfs -chmod g+w /tmp
hdfs dfs -chmod g+w /user/hive/warehouse

cd ../
bin/schematool -dbType postgres -initSchema

hive --hiveconf hive.server2.enable.doAs=false --hiveconf hive.security.authorization.enabled=false --service hiveserver

beeline -u jdbc:hive2://team-15-jn:5433

SHOW DATABASES;
CREATE DATABASE test;
DESCRIBE DATABASE test;

CREATE TABLE IF NOT EXISTS test.sleep (
    Person_ID INT,
    Gender STRING,
    Age INT,
    Occupation STRING,
    Sleep_Duration FLOAT,
    Quality_of_Sleep INT,
    Physical_Activity_Level INT,
    Stress_Level INT,
    BMI_Category STRING,
    Blood_Pressure STRING,
    Heart_Rate INT,
    Daily_Steps INT,
    Sleep_Disorder STRING
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '|';

SHOW TABLES;
DESCRIBE sleep;

hdfs dfs -mkdir -p /input
hdfs dfs -chmod g+w /input

sudo wget -O Sleep_health_and_lifestyle_dataset.csv https://raw.githubusercontent.com/nevolinaa/big_data_hw/refs/heads/main/Sleep_health_and_lifestyle_dataset.csv

hdfs dfs put Sleep_health_and_lifestyle_dataset.csv /input
hdfs fsck /input/Sleep_health_and_lifestyle_dataset.csv
head -10 /input/Sleep_health_and_lifestyle_dataset.csv

LOAD DATA INPATH  /input/'Sleep_health_and_lifestyle_dataset.csv' INTO TABLE test.sleep;

SELECT * FROM test.sleep LIMIT 10;

mapred --daemon stop historyserver
sbin/stop-yarn.sh
sbin/stop-dfs.sh
```
