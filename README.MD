Prerequisites for Apache Hive Installation:
===================================
1. HADOOP should be installed in linux system (I'm using hadoop v3.3.0). <br />
If you don't know how to install hadoop. Follow the below link for hadoop installation guide and then proceed with this tutorial. <br />
[Hadoop Installation Tutorial](https://github.com/akashpahilwan/Hadoop-Installtion-on-Linux)


Steps for Hive Installation:
==================================
1. Downloading and Unzipping Hive
2. Editing the Bashrc File
3. Editing the hive-config.sh File
4. Creating the Hive directories in HDFS
5. Fixing guava problem – Additional step
6. Initializing the DataBase type in HIVE
7. Optional Step – Editing hive-site.xml
8. Running and Configuring HIVE

Step 1- Downloading and Unzipping Hive:
=============================

I will be downloading HIVE v3.1.2 which is compatible with HADOOP v3.3.0.<br />
Run the below command while being in the root directory in which the hadoop is installed to download the HIVE gz package. <br />

```
wget https://downloads.apache.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
```

Extract the gz package with below command.<br />
```
tar xzf apache-hive-3.1.2-bin.tar.gz
```

Step 2- Editing the Bashrc File:
========================
We will be adding HIVE location(which is prsent in local system) to .basrc file. <br />
NOTE: This process is same as adding Environment variable in Windows. <br />

While being in the root directory,First open .basrc file.<br />
```
sudo nano .bashrc
```
Add the below lines of code to the end of the file.<br />
```
export HIVE_HOME=/home/hdoop/apache-hive-3.1.2-bin
export PATH=$PATH:$HIVE_HOME/bin
```

Now press Ctrl + X and then press Y.<br />
To reload the changes from .bashrc file:
```
source ~/.bashrc
```
Step 3-Editing the hive-config.sh File:
===================================

Run the below command to open hive-config.sh.
```
sudo nano $HIVE_HOME/bin/hive-config.sh
```
Add the below lines of code to the end of the file.<br />
```
export HADOOP_HOME=/home/hdoop/hadoop-3.3.0
```
NOTE: Here hdoop is the user wher I have installed hadoop and will be installing hive as well.

Step 4-Creating the Hive directories in HDFS:
===================================

Please make sure that hadoop is running before executing below commands one by one. <br />
```
hdfs dfs -mkdir /tmp
hdfs dfs -chmod g+w /tmp  
hdfs dfs -mkdir -p /user/hive/warehouse
hdfs dfs -chmod g+w /user/hive/warehouse
```


Step 5- Fixing guava problem – Additional step
===================================
Removing the guava jar file from hive installtion location.<br />
```
rm $HIVE_HOME/lib/guava-19.0.jar
```
Copyting the guava jar file from HADOOP to HIVE installtion location.
```
cp $HADOOP_HOME/share/hadoop/hdfs/lib/guava-27.0-jre.jar $HIVE_HOME/lib/
```

Step 6- Initializing the DataBase type in HIVE
============================
SETting data DataBase type as Derby.
```
schematool -initSchema -dbType derby
```


Step 7- Optional Step – Editing hive-site.xml
===========
Navigating to conf folder in hive installation location.
```
cd $HIVE_HOME/conf
```
```
cp hive-default.xml.template hive-site.xml
```
Now in  hive-site.xml : Change metastore location to above created hdfs path(/user/hive/warehouse)

Step 8- Running and Configuring HIVE
=================================
Now run the below command to start HIVE service 
```
hive
```
NOTE: If you face any issues related to SemanticException run the below commands one by one in hive shell and try again.
```
SET hive.support.concurrency=true;
SET hive.enforce.bucketing=true;
SET hive.exec.dynamic.partition.mode=nonstrict;
SET hive.txn.manager=org.apache.hadoop.hive.ql.lockmgr.DbTxnManager;
SET hive.compactor.initiator.on=true;
SET hive.compactor.worker.threads=2;
```


Congratulations you have successfully installed HIVE and started HIVE shell.
