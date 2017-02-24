---
title:  "<font color='red'>[原创]</font> 大数据平台单机搭建笔记(3)-Spark on Yarn"
date:   2016-12-04 00:00:00
categories: [原创,大数据]
tags: [原创,大数据]
---

**

## 安装Scala（版本2.11.8）
---
1. 下载Scala二进制包，解压至/opt/:

		$ tar -zxvf ./scala-2.11.8.tgz -C /opt/

## 安装Spark（版本2.1.0）
---

#### 准备工作 
1. 下载Spark二进制包（或源码自行编译），解压至/opt/:

		$ tar -zxvf ./spark-2.1.0-bin-hadoop2.7 -C /opt/
		$ mv /opt/spark-2.1.0-bin-hadoop2.7 /opt/spark-2.1.0

#### 配置Spark
1. conf/spark-env.sh:

		export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_101.jdk/Contents/Home
		export HADOOP_HOME=/opt/hadoop-2.7.3
		export SCALA_HOME=/opt/scala-2.11.8	

2. conf/slaves:

		localhost

#### 启动Spark	
1. 启动Hadoop
		
		$ hadoop-daemon.sh start namenode
		$ hadoop-daemon.sh start datanode
		$ start-yarn.sh

1. 启动Spark，用jps查看，启动的进程应包括Master、Worker:

		$ /opt/spark-2.1.0/sbin/start-all.sh

2. 测试，运行SparkPi例子:

		$ cd /opt/spark-2.1.0
		$ ./bin/spark-submit --class org.apache.spark.examples.SparkPi \
		--master yarn-cluster \
		--num-executors 3 \
		--driver-memory 2g \
		--executor-memory 1g \
		--executor-cores 1 \
		--queue thequeue \
		examples/jars/spark-examples_2.11-2.1.0.jar \
		10

3. 查看结果:

		$ yarn logs -applicationId ${applicationId} | grep 'Pi'

