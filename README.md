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


<!--Install homebrew <https://brew.sh/>
Check Java version
$ java -version
If you do not have Java8, install it
$ brew cask install homebrew/cask-versions/adoptopenjdk8-->

## Note on hadoop install 
If hadoop installation via brew does not work, get the latest stable release from:

<https://mirrors.whoishostingthis.com/apache/hadoop/common/>

<!-- have not added the JAVA_HOME setup on hadoop-env.sh as it gave error-->

# Java

`hadoop classpath` will give you the requisite libraries if you are compiling
from the command line.


<!--Follow the instructions here to make and run the jar
<http://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html>
Generate a sample input file as in 
https://docs.deistercloud.com/content/Technology.50/Hadoop/Hadoop%20single.10.xml?embedded=true#c5ceb5c977c8c4cf1d80d5601f43f406
`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep words.txt output 'mark'`-->

<!-- my hadoop is at /usr/local/Cellar/hadoop-3.2.1/ -->

`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep input output 'dfs[a-z.]+'`

## If Datanode does not start

Clean tmp directory

`$ rm -Rf /tmp/hadoop-yourusername/*`

Format namenode and then start dfs again.


# Python

Install MRJob library

https://pypi.org/project/mrjob/#description

<!-- Run the mrjob.py program
<https://pythonhosted.org/mrjob/guides/quickstart.html>
Alter the example program to produce a wordcount -->

## Bonus items: come back to this after the Lecture on Amazon EC2 and Elastic MapReduce (EMR)

* Run this on Elastic Mapreduce (You'll need API keys from your AWS Console)

