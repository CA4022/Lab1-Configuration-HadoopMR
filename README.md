# FirstLab
This is the first lab, installing hadoop, writing and running a basic hadoop program

This is the basic wordcount program as discussed in the lecture.

<!--Vagrantfile above creates an blank ubuntu trusty 64 vm and installs mrjob (python mapreduce library)-->


## Set up a single node Hadoop Cluster

Download Hadoop and run the example single-cluster grep application.
<https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html>

You might need to install oracle java, or to ensure that the ports are listening correctly.

# Python

Install MRJob library
Run the mrjob.py program

<https://pythonhosted.org/mrjob/guides/quickstart.html>

Alter the example program to produce a wordcount

# Java

`hadoop classpath` will give you the requisite libraries if you are compiling
from the command line.

Follow the instructions here to make and run the jar

<http://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html>

## Bonus items: come back to this after the Lecture on Amazon EC2 and Elastic MapReduce (EMR)

* Run this on Elastic Mapreduce (You'll need API keys from your AWS Console)

