# Lab 1 - Hadoop and Map Reduce
This lab spans across the first two weeks and it includes instructions on installing hadoop, running a single node hadoop cluster and writing/running a basic hadoop program (in Java and Python)

We will start from the basic wordcount program as illustreated in the lecture slides.

## Note on Vagrant (for VirtualBox users)
 
Vagrantfile above creates an blank ubuntu trusty 64 virtual machine and installs mrjob (python mapreduce library).
In order to avoid overload we are not going to create a VM to run Hadoop, but we will run a single node cluster on our own machine (Mac or Windows) or on the Unix Lab machine accessible remotely.

<!--Note on setting your path: https://stackabuse.com/how-to-permanently-set-path-in-linux/-->

# Windows users
Windows users have two options:

## 1. Installing a Virtual Machine (running Ubuntu)
Instructions as follows:

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

## 2. Use Windows Subsystem for Linux (WSL) on Win10

1. Install Ubuntu using [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install-win10)


## NOTE on configuration of ports

You might need to change the port used for localhost based on your system.

<!--Install homebrew <https://brew.sh/>
Check Java version
$ java -version
If you do not have Java8, install it
$ brew cask install homebrew/cask-versions/adoptopenjdk8-->



# Downloading and installing Hadoop from command line

1. Download binary from the internet (using a mirror site)

`$ wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz`

2. Unzip hadoop binary (in current directory, use `$ -C ~/<dirname> ` to unzip in a different directory `<dirname>` under user home folder)

`$ tar -xvzf hadoop-3.3.0.tar.gz `


## MAC OS X only
I found these step-by-step guides works with very minor issues (includes running a single node cluster):

1. [Jan2020 guide](https://towardsdatascience.com/installing-hadoop-on-a-mac-ec01c67b003c)
<!-- 2. <https://www.digitalcare.org/how-to-install-hadoop-on-mac/ -->
2. [Hadoop 3.3.0](https://techblost.com/how-to-install-hadoop-on-mac-with-homebrew/)


## NOTE on hadoop install vith homebrew (MAC OS users ONLY)
If hadoop installation via brew does not work, get the latest stable release from:

<https://mirrors.whoishostingthis.com/apache/hadoop/common/>

<!-- have not added the JAVA_HOME setup on hadoop-env.sh as it gave error-->


# Set up and run a single node Hadoop Cluster (Standalone mode)

Now you are either working on your Linux VM, or on your Linux/Unix/OS X machine
The official Apache manual for installing a single node Hadoop Cluster is here:

<https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html>

Follow instructions from **Prepare to start the Hadoop Cluster**

## Note on how to check everything is in the right place

* Where is my JAVA_HOME? `$ /usr/libexec/java_home`
* How do I check what is the current setup for JAVA_HOME? `$ echo $JAVA_HOME`
* How do I set my JAVA_HOME? 
   1. For this session only: `$ export JAVA_HOME=<path returned from previous command>`
   2. For hadoop: change value in configuration file `etc/hadoop/hadoop-env.sh`
   3. Permanently:
          * UNIX/LINUX: Modify file ~/.bashrc in unix
          * MACOS X (bash shell): Modify file ~/.bash_profile 
          * MACOS X (zsh shell): Modify file ~/.zshenv (from Catalina)

* How do I make changes to env variables effective?
   1. run `$ source ~/.bash_profile` or `$ source ~/.zshenv`
   2. restart terminal

* How to check what env variables are set? Run `$ printenv `

## Start your cluster
Remember you need to start your cluster before you can access anything on the Hadoop Distributed File System (HDFS).
Make sure you are in hadoop home directory`$ cd <hadoop dir> `

Follow instructions from **Execution** in the Apache [manual](<https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html>)

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

* make sure you set your JAVA_HOME to the right `<path>` (see above).
<!-- You can find this path by running 

----`$ /usr/libexec/java_home` on my MAC ----

`$ dirname $(dirname $(readlink -f $(which javac)))`    (on Linux/Unix)

`$ $(dirname $(readlink $(which javac)))/java_home`    (on MAC OS)

and set it by using `$ export JAVA_HOME=<path>`-->

* make sure you set your HADOOP_CLASSPATH: 

`$ export HADOOP_CLASSPATH=$JAVA_HOME/lib/tools.jar`

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

* start the cluster again: 
  - `$ sbin/start-dfs.sh`
  - `$ sbin/start-yarn.sh`

If this does not work:

* stop the cluster: 
`$ sbin/stop-all.sh`

* format namenode (note this will remove all directories and files you have created in your hdfs!):
`$ bin/hdfs namenode -format`

* start the cluster again: 
  - `$ sbin/start-dfs.sh`
  - `$ sbin/start-yarn.sh`


# Python
Hadoop Streaming allows you to create and run Map/Reduce jobs with any executable or script as the mapper and/or the reducer.
For Python, the MRJob Library wraps Hadoop streaming and allows you to run python mapreduce jobs.

See <https://pypi.org/project/mrjob/> for more info.

Install MRJob using pip (or pip3 depending on your Python version) as in the tutorial below:

<https://github.com/Yelp/mrjob/blob/master/docs/guides/quickstart.rst>

## Note: Python MapReduce via Hadoop Streaming

If you want to try Hadoop Streaming to run Python (but not only) jobs, the tutorial below is a good starting point:

https://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/


<!--Install MRJob library
https://pypi.org/project/mrjob/#description
Run the mrjob.py program
<https://pythonhosted.org/mrjob/guides/quickstart.html>
Alter the example program to produce a wordcount -->

# Bonus: run on a Cloud Platrofm (AWS EMR or Google DataProc)

We will come back to this after the Lecture on Amazon EC2 and Elastic MapReduce (EMR).
Labsheet available [here](https://github.com/CA4022/Big-Data-Cloud)

**Note for AWS only**: to run this on Elastic Mapreduce you'll need API keys from your AWS Console.


