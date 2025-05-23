# Spark

## Installation

As `hadoop`, download, extract and rename the newest Spark binaries:

```bash
cd ~
wget https://dlcdn.apache.org/spark/spark-3.5.5/spark-3.5.5-bin-hadoop3.tgz
tar xvf spark-3.5.5-bin-hadoop3.tgz
mv spark-3.5.5-bin-hadoop3 spark
```

## Bashrc

As always, we want to have the Spark path in our `PATH` variable.

Let's add the following block to the **end** of `~/.bashrc`:

```text
# Spark
export SPARK_HOME=/home/hadoop/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
```

```bash
nano ~/.bashrc
```

Source `~/.bashrc` to load the configuration we just made:

```bash
source ~/.bashrc
```

## spark-defaults.conf

We can adjust the default behavior for Spark with `spark-defaults.conf`

```bash
cd ~/spark/conf/
```

```bash
cp spark-defaults.conf.template spark-defaults.conf
```

Let's add the following block to the **end** of `spark-defaults.conf`:

```text
spark.master yarn

spark.eventLog.enabled true
spark.history.ui.port 18080

spark.yarn.am.memory 3g
spark.executor.memory 2g

spark.executor.instances 6
spark.executor.cores 3

spark.yarn.am.cores 2
```

```bash
nano spark-defaults.conf
```

## spark-env.sh

Environments variables for Spark can be set via `spark-env.sh`

```bash
cd ~/spark/conf/
```

Create a new file called `spark-env.sh`

```bash
touch spark-env.sh
```

Give executable permission with:

```bash
chmod 755 spark-env.sh
```

and add the following content to it:

```bash
#!/usr/bin/env bash
HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HADOOP_HOME/lib/native
SPARK_CLASSPATH=$HIVE_HOME/lib/mysql-connector-java.jar

SPARK_LOCAL_HOSTNAME=$HOSTNAME.bdlc.ls.eee.intern
SPARK_IDENT_STRING=$HOSTNAME.bdlc.ls.eee.intern
SPARK_PUBLIC_DNS=$HOSTNAME.bdlc.ls.eee.intern
```

```bash
nano spark-env.sh
```

## Logging

By default, Spark is very verbose. Let's change this by modifying the `log4.properties` file.

Still in the `conf` directory:

```bash
cd ~/spark/conf/
```

Copy the template version to the actual file:

```bash
cp log4j2.properties.template log4j2.properties
```

Change the line from:

```text
rootLogger.level = info
```

to:

```text
rootLogger.level = warn
```

by using `nano`:

```bash
nano log4j2.properties
```

## History Server

In order to keep all logs from Spark, we will start the `history server` with:

```bash
mkdir /tmp/spark-events
start-history-server.sh
```

P.S. It should be no surprise that you can stop it with `stop-history-server.sh`

## Testing

Let us test if the Spark installation works in the CLI.

Check out:

- The [Resource Manager](http://bdlc-XXX.bdlc.ls.eee.intern:8088/cluster)
- The [Spark History Server](http://bdlc-XXX.bdlc.ls.eee.intern:18080/)

When you perform these tasks.

### Pi with spark-submit

```bash
spark-submit --deploy-mode client --class org.apache.spark.examples.SparkPi /home/hadoop/spark/examples/jars/spark-examples_2.12-3.5.5.jar 10
```

### Pyspark - Interactive Shell

```bash
pyspark
```

You can see in the Web UIs that the session remains open.

Let us compute our first `Word Count` with Spark.

```python
path = "/dataset/text/holmes.txt"
lines = sc.textFile(path)

words = lines.flatMap(lambda line: line.split(" "))

word_one_pairs = words.map(lambda word: (word, 1))

word_count = word_one_pairs.reduceByKey(lambda x, y: x + y)

word_count.take(5)
```

Exit with `exit()`.

## References

- https://spark.apache.org
- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://computingforgeeks.com/how-to-install-apache-spark-on-ubuntu-debian/
- https://sparkbyexamples.com/spark/spark-setup-on-hadoop-yarn/
