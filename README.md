# Dockerizing an Apache Spark Standalone Cluster

### Check the article here:  <a href="https://medium.com/geekculture/dockerizing-an-apache-spark-standalone-cluster-eeb7d3f8efeb">Dockerizing an Apache Spark Standalone Cluster</a>

## Small Big Data ecosystem with In-Memory Processing

The aim of this repository is to show you the use of docker as a powerful tool to deploy applications that communicate with each other in a fast and stable way. In this case I present a small ecosystem of big data for in-memory processing, the ecosystem is based in different docker containers, it was considered to add Apache Hive and Hue as needed applications in a simple big data ecosystem with Apache Spark, this project will help you go directly to writing scala or pyspark code and get hands-on experience and not worry about the installation and implementation of the infrastructure, the installed applications and the reason for each one them are detailed below.

# Docker containers architecture
![architecture](https://user-images.githubusercontent.com/8701464/127952650-c71d6374-3cb0-40fc-8df5-01ffda530081.png)

## Apache Spark 
Apache Spark itself does not supply storage or any Resource Management. It is just a unified framework for in memory processing large amount of data near to real time.
checking the docker-compose.yml file you can see the detailed docker images for spark

- spark-master (wittline/spark-master:3.0.0)
- spark-worker-1 (wittline/spark-worker:3.0.0)
- spark-worker-2 (wittline/spark-worker:3.0.0)
 
you can check the details about the docker image here: <a  href="https://hub.docker.com/u/wittline"> wittline</a>

### What is a standalone cluster?
There are different ways to run an Apache Spark application: Local Mode, Standalone mode, Yarn, Kubernetes and Mesos, these are the way how Apache spark assigns resources to its drivers and executors, the last three mentioned are cluster managers, everything is based on who is the master node, let's see the table below:

![stdmode](https://user-images.githubusercontent.com/8701464/128092104-12c8c50c-992c-45d9-ba5e-27e47ab2af34.png)

The table above shows that one way to recognize a standalone configuration is by observing who is the master node, a standalone configuration can only run applications for apache spark and submit spark applications directly to the master node.

## JupyterLab
The pyspark code will be written using jupyter notebooks, we will submit the code to the standalone cluster using the SparkSession
checking the docker-compose.yml file you can see the detailed docker images for spark

- jupyterlab (wittline/jupyterlab:3.0.0-spark-3.0.0)

you can check the details about the docker image here: <a  href="https://hub.docker.com/u/wittline"> wittline</a>

## Hive
Apache Spark manages all the complexities of create and manage global and session-scoped views and SQL managed and unmanaged tables, in memory and disk, and SparkSQL is one the main components of Apache Spark, integrating relational procesing with spark functional programming. Apache Spark by default uses the Apache Hive metastore, located at user/hive/warehouse, to persist all the metadata about the tables created. Apache Spark does not need Hive, the idea of adding hive to this architecture is to have a metadata storage about tables and views that can be reused in a workload and thus avoid the use of re-creating queries for these tables. For example: A global temporary view is visible accross multiple SparkSessions, in this case we can conbine data from different SparkSessions that do not share the same hive metastore. Hive data warehouse facilitates reading, writing, and managing large datasets on HDFS storage using SQL

- hive-server (fjardim/fjardim/hive)
- hive-metastore (wittline/fjardim/hive)
- hive-metastore-postgresql (wittline/spark-worker:3.0.0)

you can check the details about the docker image here: <a href="https://hub.docker.com/u/fjardim">fjardim</a>


## Namenode and datanodes (HDFS)
The Namenode is the master node which persist metadata in HDFS and the datanode is the slave node which store the data. When you insert data or create objects into Hive tables, data will be stored in HDFS on Hadoop DataNodes and the NameNode will keep the tracking of which DataNode has the data.

- namenode (fjardim/namenode_sqoop)
- datanode1 (fjardim/datanode)
- datanode2 (fjardim/datanode)

you can check the details about the docker image here: <a href="https://hub.docker.com/u/fjardim">fjardim</a>

## Hue

Hue is an open source SQL Assistant for Databases & Data Warehouses, It is not necessary for a big data ecosystem, but it can help you visualize data in HDFS faster, and other notable features.

- namenode (fjardim/hue)
- database(fjardim/mysql)

you can check the details about the docker image here: <a href="https://hub.docker.com/u/fjardim">fjardim</a>

## How to run the docker environment
- Install Docker Desktop on Windows, it will install Docker Compose as well, Docker Compose will allow you to run multiple container applications
Install git-bash for windows, once installed , open git bash and download the below repository, this will download all the files needed.

``` 
ramse@DESKTOP-K6K6E5A MINGW64 /c
$ git clone https://github.com/Wittline/apache-spark-docker.git
```

- once inside the folder use the below comman, this will install all the images from the docker-compose file and will setup all the containers, This will take some time.

```
ramse@DESKTOP-K6K6E5A MINGW64 /c/apache-spark-docker/docker
$ winpty docker-compose up -d
```

- When everything is finished, you will see the name of all the containers with the status done
- Now go to jupyterlab, using the url: http://localhost:8889/, this will open a new tab, enjoy writing your pyspark code.
- Go to Hue using the url: http://localhost:8888/, and check your tables in HDFS

## Contributing and Feedback
Any ideas or feedback about this repository?. Help me to improve it.

## Authors
- Created by <a href="https://www.linkedin.com/in/ramsescoraspe"><strong>Ramses Alexander Coraspe Valdez</strong></a>
- Created on August, 2021

## License
This project is licensed under the terms of the Apache License.

