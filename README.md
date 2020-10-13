# FirstLab
This is the first lab, installing hadoop, writing and running a basic hadoop program.

We will start from the basic wordcount program as illustreated in the lecture slides.

## Note on Vagrant
 
Vagrantfile above creates an blank ubuntu trusty 64 virtual machine and installs mrjob (python mapreduce library).
In order to avoid overload we are not going to create a VM to run Hadoop, but we will run a single node cluster on our own machine (Mac or Windows) or on the Unix Lab machine accessible remotely.


# Set up a single node Hadoop Cluster

The official Apache manual for installing a single node Hadoop Cluster is here:

<https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html>

For MAC users, I found this step-by-step guide works with very minor issues:

<https://towardsdatascience.com/installing-hadoop-on-a-mac-ec01c67b003c>

## Note on configuration files

You mght need to change the port used for localhost based on your system.

<!--Install homebrew <https://brew.sh/>
Check Java version
$ java -version
If you do not have Java8, install it
$ brew cask install homebrew/cask-versions/adoptopenjdk8-->

## Note on hadoop install 
If hadoop installation via brew does not work, get the latest stable release from:

<https://mirrors.whoishostingthis.com/apache/hadoop/common/>

<!-- have not added the JAVA_HOME setup on hadoop-env.sh as it gave error-->

# Java: running an example jar

Follow the instructions here for execution of a jar:

<https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html#Standalone_Operation>

In particular, set your HDFS input and copy input files in there:

`$ bin/hdfs dfs -mkdir input`

`$ bin/hdfs dfs -put etc/hadoop/*.xml input`


`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep input output 'dfs[a-z.]+'`

or 

`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount input output`

* visualise the output

`$ bin/hdfs dfs -cat output/part-r-00000`

## Note: available example
The use of wordcount and grep is an example, you can see available example by running

`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar xxx input output`

## NOTE on output folder

If you run a different jar, remember to use a different output folder or to remove the existing output

`$ bin/hdfs dfs -rm -R output `

<!--Generate a sample input file as in 
https://docs.deistercloud.com/content/Technology.50/Hadoop/Hadoop%20single.10.xml?embedded=true#c5ceb5c977c8c4cf1d80d5601f43f406
`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep words.txt output 'mark'`
`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep input output 'dfs[a-z.]+'` -->

<!-- my hadoop is at /usr/local/Cellar/hadoop-3.2.1/ -->

# Java: compile your own java program

`hadoop classpath` will give you the requisite libraries if you are compiling
from the command line.

Follow the instructions here to make and run the jar

<http://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html>

If you want to compile and run your own java program (note the cluster must be running):

* make sure you set your JAVA_HOME to the right path (you find it by running 
`$ /usr/libexec/java_home`)

* make sure you set your HADOOP_CLASSPATH: 

`$export HADOOP_CLASSPATH=$JAVA_HOME/lib/tools.jar`

* after creating and saving your WordCount.java program, compile it and create a jar file

`$ bin/hadoop com.sun.tools.javac.Main WordCount.java`
`$ jar cf wc.jar WordCount*.class`

* execute it on HDFS: 
`$ bin/hadoop jar wc.jar WordCount input output`


# If Datanode does not start

* clean tmp directory: 
`$ rm -Rf /tmp/hadoop-yourusername/*`

* format namenode:
`$ bin/hdfs namenode -format`

* start the cluster again: 
`$ sbin/start-all.sh`

# Python: try it 
You can also run MapReduce job on Hadoop written in Python.
The tutorial below is a good starting point to try this:

https://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/

<!--Install MRJob library
https://pypi.org/project/mrjob/#description
Run the mrjob.py program
<https://pythonhosted.org/mrjob/guides/quickstart.html>
Alter the example program to produce a wordcount -->

# Bonus: run on EMR
We will come back to this after the Lecture on Amazon EC2 and Elastic MapReduce (EMR)

To run this on Elastic Mapreduce you'll need API keys from your AWS Console.

