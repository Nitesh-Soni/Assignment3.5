1. Explain what is High availability of Namenode.
The HDFS NameNode High Availability feature enables you to run redundant NameNodes in the same cluster in an Active/Passive configuration with a hot standby. This eliminates the NameNode as a potential single point of failure (SPOF) in an HDFS cluster.

In previous version, if a cluster had a single NameNode, and that machine or process became unavailable, the entire cluster would be unavailable until the NameNode was either restarted or started on a separate machine. This situation affected the total availability of the HDFS cluster in two major ways:
1.	In the case of an unplanned event such as a machine crash, the cluster would be unavailable until an operator restarted the NameNode.
2.	Planned maintenance events such as software or hardware upgrades on the NameNode machine would result in periods of cluster downtime.
HDFS NameNode avoids this by facilitating either a fast failover to the new NameNode during machine crash, or a graceful administrator-initiated failover during planned maintenance.

2. Explain what is check pointing and how it is useful.
Checkpointing is an important part of maintaining filesystem metadata in HDFS. For efficient NameNode recovery for an unwanted failure it is important, and it is an indicator of overall cluster health. However, checkpointing can also be a source of confusion for operators of Apache Hadoop clusters.
Checkpointing is a process in which the secondary node that takes an fsimage and edit log from active node and merge them into a new fsimage and send back to the active node. This way, the NameNode can load the final in-memory state directly from the fsimage. This results in more efficient operation and reduces NameNode startup time.
1.	When it’s time to perform the checkpoint, the NameNode creates a new file to accept the journal file system changes.
It names the new file.
2.	As a result, the file accepts no further changes and is copied to the checkpointing service, along with the file.
3.	The checkpointing service merges these two files, creating a file named.
4.	The checkpointing service copies the file to the NameNode.


3. Explain what is HDFS federation 
Hadoop federation allows scaling the name service horizontally. It uses several namenodes or namespaces which are independent of each other. These independent namenodes don’t require inter coordination. These datanodes are used as common storage by all the namenodes. Each datanode is registered with all the namenodes in the cluster. These datanodes send periodic reports and responds to the commands from the name nodes. We have a block pool which is a set of blocks that belong to a single namespace. In a cluster, the datanodes stores blocks for all the block pools. Each block pool is managed independently. This enables the name space to generate block ids for new blocks without informing other namespaces. If one namenode fails for any reason, the datanode keeps on serving from other namenodes.

One namespace and its block are collectively called Namespace Volume. When a namespace or a namenode is deleted the corresponding block pool at the datanode is deleted automatically. In the process of cluster up-gradation, each namespace volume is upgraded as a unit.

5.	What are the configuration files that are to be edited for sure while installing a Hadoop cluster
The $HADOOP_INSTALL/hadoop/conf directory contains some configuration files for Hadoop. These are:
•	hadoop-env.sh - This file contains some environment variable settings used by Hadoop. You can use these to affect some aspects of Hadoop daemon behavior, such as where log files are stored, the maximum amount of heap used etc. The only variable you should need to change in this file is JAVA_HOME, which specifies the path to the Java 1.5.x installation used by Hadoop.
•	slaves - This file lists the hosts, one per line, where the Hadoop slave daemons (datanodes and tasktrackers) will run. By default this contains the single entry localhost
•	hadoop-default.xml - This file contains generic default settings for Hadoop daemons and Map/Reduce jobs. Do not modify this file.
•	mapred-default.xml - This file contains site specific settings for the Hadoop Map/Reduce daemons and jobs. The file is empty by default. Putting configuration properties in this file will override Map/Reduce settings in the hadoop-default.xml file. Use this file to tailor the behavior of Map/Reduce on your site.
•	hadoop-site.xml - This file contains site specific settings for all Hadoop daemons and Map/Reduce jobs. This file is empty by default. Settings in this file override those in hadoop-default.xml and mapred-default.xml. This file should contain settings that must be respected by all servers and clients in a Hadoop installation, for instance, the location of the namenode and the jobtracker.

