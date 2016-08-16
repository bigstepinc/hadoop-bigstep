# Bigstep DataLake client libraries

These libraries enable the "dl://" prefix in hadoop and associated tools so that hdfs dfs -ls or distcopy work properly. They can also be used as a standalone FileSystem implementation to enable easy interaction with the datalake from java or scala applications.

To use as part of a hadoop stack, first copy the jar to:

1. Vanilla Hadoop  **hadoop-2.7.x/share/hadoop/common/**
2. Vanilla Spark 2.0 **spark-2.x.x/jars**
3. Cloudera CDH 5.x  **/opt/cloudera/parcels/CDH/lib/hadoop/**

It should be possible technically to be used with any hadoop enabled application as long as it is added to the classpath. 

Add the following to **core-site.xml**. If using Spark standalone place this file in the **spark-2.x.x/conf** directory:

```xml
<property>
  <name>fs.dl.impl</name>
  <value>com.bigstep.datalake.DLFileSystem</value>
</property>

<property>
  <name>fs.dl.impl.kerberosPrincipal</name>
  <value>kxxx@bigstep.io</value>
</property>

<property>
  <name>fs.dl.impl.kerberosKeytab</name>
  <value>/etc/hadoop/kx.keytab</value>
</property>

<property>
  <name>fs.dl.impl.kerberosRealm</name>
  <value>bigstep.io</value>
</property>

<property>
  <name>fs.dl.impl.homeDirectory</name>
  <value>/data_lake/dlxxxx</value>
</property>

<!-- optional -->
<property>
  <name>fs.dl.impl.defaultFilePermissions</name>
  <value>00640</value>
</property>

<!-- optional -->
<property>
  <name>fs.dl.impl.defaultUMask</name>
  <value>007</value>
</property>
```

Make sure that the jar is available on all the cluster machines. Also the keytab must be reachable to yarn user (eg: not /root).

To create a keytab follow the instructions provided [on the DataLake documentation](https://fullmetal.bigstep.com/docs#documents/61).

Then you can use regular hadoop commands like distcp:
```bash
hadoop distcp hdfs://localhost/user/hdfs/test dl://node10930-datanodes-data-lake01-uk-reading.bigstep.io:14000/data_lake/dlzzz
```

To use directly in a project use:
```xml
<dependency>
  <groupId>com.bigstep</groupId>
  <artifactId>datalake</artifactId>
  <version>1.0</version>
</dependency>
```

To compile use:
```bash
mvn package
```
