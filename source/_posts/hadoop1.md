---
title: hadoop初步使用
tags:
  - scala
  - hadoop
  - Big Data
date: 2018-11-23 14:47:56
---

本文仅限学习hadoop这一工具，hive、hbase、spark等之后再学习.

以下案例使用的hadoop版本号为2.7.7
## 安装hadoop
[hadoop官网][hadoop]

[清华源下载地址][tsinghua] 速度比较快

下载后解压即可

## 启动hadoop
`etc/hadoop/hadoop-env.sh`:
```
  # set to the root of your Java installation
  export JAVA_HOME="java路径"
```
<!-- more -->
### 伪分布模式
- 配置ssh能登录本机
`ssh localhost`

- 配置文件：
`etc/hadoop/core-site.xml`：
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
`etc/hadoop/hdfs-site.xml`:
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

- hdfs启动前先格式化
```
bin/hdfs namenode -format
```

- 启动hdfs
`sbin/start-dfs.sh`

## hadoop常用命令行
```
hadoop fs -ls /
hadoop fs -cat file
hadoop fs -rm file
hadoop fs -put src target
hadoop fs -get src target
```
## scala使用hadoop api
sbt依赖：
```
name := "hadoop_sbt"

version := "0.1"

scalaVersion := "2.11.12"

// https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-hdfs
libraryDependencies += "org.apache.hadoop" % "hadoop-hdfs" % "2.7.7"
// https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common
libraryDependencies += "org.apache.hadoop" % "hadoop-common" % "2.7.7"
// https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-client
libraryDependencies += "org.apache.hadoop" % "hadoop-client" % "2.7.7"
// https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-auth
libraryDependencies += "org.apache.hadoop" % "hadoop-auth" % "2.7.7"
// https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-mapreduce-client-core
libraryDependencies += "org.apache.hadoop" % "hadoop-mapreduce-client-core" % "2.7.7"
// https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-yarn-api
libraryDependencies += "org.apache.hadoop" % "hadoop-yarn-api" % "2.7.7"
```

scala操作hdfs

```scala
import org.apache.hadoop.conf.Configuration
import org.apache.hadoop.fs.{FileSystem, Path}
import org.apache.commons.io.IOUtils

object test_hdfs {
  def main(args: Array[String]): Unit = {
    // 配置
    val conf = new Configuration()
    // 读取hadoop的基本配置文件
    conf.addResource(new Path("hadoop-2.7.7/etc/hadoop/core-site.xml"))
    conf.addResource(new Path("hadoop-2.7.7/etc/hadoop/hdfs-site.xml"))
    // 文件系统类 
    val fs = FileSystem.get(conf)

    // 展示文件名
    fs.listStatus(new Path("/tmp")).map(s => println(s.getPath().getName))
    // 文件改名
    fs.rename(new Path("/tmp/text1"),new Path("/tmp/text_rename"))
    // 读取文件
    val file1 = fs.open(new Path("/tmp/text_rename"))
    val out = IOUtils.toString(file1, "UTF-8")
    println(out)
    file1.close()
    fs.close()
    // 写入文件 
    val file3=fs.create(new Path("/tmp/text3"))
    file3.write(("test\n".getBytes("UTF-8")))
    file3.writeChars("test2")
    file3.flush()
    file3.close()
  }
}

```

## scala提交map-reduce任务

wordcount任务

```scala
import java.lang.Iterable
import java.util.StringTokenizer

import org.apache.hadoop.conf.Configuration
import org.apache.hadoop.fs.Path
import org.apache.hadoop.io.{IntWritable, Text}
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat
import org.apache.hadoop.mapreduce.{Job, Mapper, Reducer}
import scala.collection.JavaConverters._


object map_reduce_submit {

  class TokenizerMapper extends Mapper[Object, Text, Text, IntWritable] {

    val one = new IntWritable(1)
    val word = new Text()

    override def map(key: Object, value: Text, context: Mapper[Object, Text, Text, IntWritable]#Context): Unit = {
      val itr = new StringTokenizer(value.toString)
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken())
        context.write(word, one)
      }
    }
  }

  class IntSumReader extends Reducer[Text, IntWritable, Text, IntWritable] {
    override def reduce(key: Text, values: Iterable[IntWritable], context: Reducer[Text, IntWritable, Text, IntWritable]#Context): Unit = {
      var sum = values.asScala.foldLeft(0)(_ + _.get)
      context.write(key, new IntWritable(sum))
    }
  }

  def main(args: Array[String]): Unit = {
    val configuration = new Configuration()
    configuration.addResource(new Path("hadoop-2.7.7/etc/hadoop/core-site.xml"))
    configuration.addResource(new Path("hadoop-2.7.7/etc/hadoop/hdfs-site.xml"))
    val job = Job.getInstance(configuration, "word count")
    job.setJarByClass(this.getClass)
    job.setMapperClass(classOf[TokenizerMapper])
    job.setCombinerClass(classOf[IntSumReader])
    job.setReducerClass(classOf[IntSumReader])
    job.setOutputKeyClass(classOf[Text])
    job.setOutputKeyClass(classOf[Text])
    job.setOutputValueClass(classOf[IntWritable])
    FileInputFormat.addInputPath(job, new Path("/tmp/text2"))
    FileOutputFormat.setOutputPath(job, new Path("/tmp/text_result"))
    System.exit(if (job.waitForCompletion(true)) 0 else 1)
  }
}

```

[hadoop]: http://hadoop.apache.org/
[tsinghua]: https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/