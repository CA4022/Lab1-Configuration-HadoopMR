# FirstLab
This is the first lab, installing hadoop, writing and running a basic hadoop program.

We will start from the basic wordcount program as illustreated in the lecture slides.

## Note on Vagrant
 
Vagrantfile above creates an blank ubuntu trusty 64 virtual machine and installs mrjob (python mapreduce library).
In order to avoid overload we are not going to create a VM to run Hadoop, but we will run a single node cluster on our own machine (Mac or Windows) or on the Unix Lab machine accessible remotely.


# Windows users
Windows users would need to install a Virtual Machine running Linux, instructions as follows:

1. Install [VirtualBox](https://www.virtualbox.org/)
2. Install [Vagrant](https://www.vagrantup.com/)
3. CD into the directory where you want to put your VagrantFile
    * Note for DCU lab machine users - When creating a VM, you should create a folder in d:\virtual (or d:\temp) and store the VM there (or on a personal USB hard drive)
4. `vagrant --version`will check your vagrant version 
5. `vagrant init ubuntu/trusty64` will create a vagrant box (and your vagrant file) and set up your virtual machine
    * Note you can only have one VagrantFile in any given directory
6. `vagrant up` will start the process of downloading and configuring the VM
7. Use `vagrant ssh` to ssh into the running machine
    * Alternatively, you can use putty to ssh into `localhost` port `2222` (username/password is `vagrant`)
8. You can then open the `VagrantFile` in a text editor and edit as necessary (but we don't have to do that)
    * You might wish to change the number of VCPUs, Memory Allocation
    * You might wish to add or remove shared directories
    * For more details on Vagrantfile commands see https://www.vagrantup.com/docs/vagrantfile/
    * During the course of the labs for this module we can add commands to the vagrant file, building upon the blank intial VagrantFile to create one which provisions the VM with everything you need already installed.
9. `vagrant halt` shuts down the VM
10. `vagrant destroy` deletes the VM 

* NOTE: careful with `vagrant destroy`! If you have installed software on that VM, everything will be deleted, unless... you have updated your vagrant file and saved a copy of it. You will still loose the data)

Windows tutorial for above instructions here:

<https://sloopstash.com/blog/how-to-build-vm-on-windows-10-using-virtualbox-vagrant-git-bash.html>

## NOTE on configuration files

You might need to change the port used for localhost based on your system.

<!--Install homebrew <https://brew.sh/>
Check Java version
$ java -version
If you do not have Java8, install it
$ brew cask install homebrew/cask-versions/adoptopenjdk8-->


# Set up a single node Hadoop Cluster (Unix/Linux)

Now you are either working on your Linux VM, or on your Linux/Unix/OS X machine
The official Apache manual for installing a single node Hadoop Cluster is here:

<https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html>

# Downloading and installing Hadoop from command line

1. Download binary from the internet (using a mirror site)

`$ wget https://mirrors.whoishostingthis.com/apache/hadoop/common/stable/hadoop-3.2.1.tar.gz`

2. Unzip hadoop binary (in current directory, use `$ -C ~/<dirname> ` to unzip in a different directory `<dirname>` under user home folder)

`$ tar -xvzf hadoop-3.3.0.tar.gz `


## MAC OS X only
I found this step-by-step guide works with very minor issues:

<https://towardsdatascience.com/installing-hadoop-on-a-mac-ec01c67b003c>

## NOTE on hadoop install vith homebrew (MAC OS users ONLY)
If hadoop installation via brew does not work, get the latest stable release from:

<https://mirrors.whoishostingthis.com/apache/hadoop/common/>

<!-- have not added the JAVA_HOME setup on hadoop-env.sh as it gave error-->

# Java: running an example jar

Follow the instructions here for execution of a jar:

<https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html#Standalone_Operation>

In particular: 

* Set your HDFS input and copy input files in there:

`$ bin/hdfs dfs -mkdir input`

`$ bin/hdfs dfs -put etc/hadoop/*.xml input`

* Run a jar (compiled java file) from the mapreduce examples available in your hadoop folder:

`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep input output 'dfs[a-z.]+'`

or 

`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount input output`

* Visualise the output

`$ bin/hdfs dfs -cat output/part-r-00000`

or

`$ bin/hdfs dfs -cat output/* `

## NOTE on available example
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

## Note on file location
Make sure files you create need to be copied into HDFS root in order to be found.

Check step 9 of the tutorial available here <https://docs.deistercloud.com/content/Technology.50/Hadoop/Hadoop%20single.10.xml?embedded=true#c5ceb5c977c8c4cf1d80d5601f43f406>

# If Datanode does not start

* clean tmp directory: 
`$ rm -Rf /tmp/hadoop-yourusername/*`

* format namenode:
`$ bin/hdfs namenode -format`

* start the cluster again: 
`$ sbin/start-all.sh`

# Python
Hadoop Streaming allows you to create and run Map/Reduce jobs with any executable or script as the mapper and/or the reducer.
For Python, the MRJob Library wraps Hadoop streaming and allows you to run python mapreduce jobs.

See <https://pypi.org/project/mrjob/> for more info.

Install MRJob using pip (or pip3 depending on your Python version) as in the tutorial below:

<https://github.com/Yelp/mrjob/blob/master/docs/guides/quickstart.rst>

## Note: Python MapReduce via Hadoop Streaming

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

