# A working virtual [spark](http://spark.apache.org/) cluster

This vagrant script allows you to set up a virtual spark cluster to practice working with spark in distributed mode. The code for this set up is a re-mix of the [ipython-spark-docker](https://github.com/Lab41/ipython-spark-docker) project by [Lab41](https://www.lab41.org/) and the [virtual-haddop-cluster](https://blog.cloudera.com/blog/2014/06/how-to-install-a-virtual-apache-hadoop-cluster-with-vagrant-and-cloudera-manager/) by [cloudera](http://www.cloudera.com/)

## Specs

The cluster consists of 4 nodes:

* Master node with 2GB of RAM (Running the Spark Scheduler)
* 3 slaves with 2GB of RAM each (Running DataNodes)

As you can see, you'll need at least 8GB of free RAM to run this. If you have less, you can try to remove one machine from the Vagrantfile. This will lead to worse performance though!

## Usage

Depending on the hardware of your computer, installation will probably take between 5 and 10 minutes.

First install [VirtualBox](https://www.virtualbox.org/) and [Vagrant](http://www.vagrantup.com/).

Install the Vagrant [Hostmanager plugin](https://github.com/smdahlen/vagrant-hostmanager)

```bash
$ vagrant plugin install vagrant-hostmanager
```

Clone this repository.

```bash
$ git clone https://github.com/jshiv/virtual-spark-cluster.git
```

Provision the bare cluster. It will ask you to enter your password, so it can modify your `/etc/hosts` file for easy access in your browser. It uses the Vagrant Hostmanager plugin to do this.

```bash
$ cd virtual-spark-cluster
$ vagrant up
```

Go to the [Spark Manager web console](http://vm-cluster-node1:7077) to see the confgured cluster.


To start up a client in python and run jobs against your cluster(assuming you are runing python on [anaconda](https://www.continuum.io/downloads)).

[Download](http://spark.apache.org/downloads.html) spark to be used as the client.

```bash
rm -rf spark-1.6.0-bin-hadoop2.4
curl -O http://www.interior-dsgn.com/apache/spark/spark-1.6.0/spark-1.6.0-bin-hadoop2.4.tgz
tar zxvf spark-1.6.0-bin-hadoop2.4.tgz
```
or
```bash
brew install apache-spark
```

Install [findspark](https://github.com/minrk/findspark).

```bash
pip install findspark
```


```python
import findspark
findspark.init()#find your spark distribution

import pyspark
sc = pyspark.SparkContext('spark://vm-cluster-node1:7077', appName = 'MyLittleCluster')

#if you have data in hdfs or s3, spark can pull the data and processes it on your cluster like this:
rdd = sc.textFile('hdfs://hdfs-host-name:8020/hdfs/path/to/data')
rdd.count()
```

to stop the cluster
```bash
vagrant suspend
```
or to remove the virtual machines completely
```bash
vagrant destroy
```

**Done!** Have fun with your Spark cluster.
