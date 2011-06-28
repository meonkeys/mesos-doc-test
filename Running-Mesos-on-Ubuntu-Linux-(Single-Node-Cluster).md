## Running Mesos On Ubuntu Linux (Single Node Cluster)
This is step-by-step guide on setting up Mesos on a single node, and running hadoop on top of Mesos. Here we are assuming Ubuntu 10.04 LTS - Long-term support 64-bit (Lucid Lynx).  Plus, we are using username "hadoop" for this guide.

## Prerequisites:
* Java
    For Ubuntu 10.04 LTS (Lucid Lynx):  
    - Go to Applications > Accessories > Terminal.
    - Create/Edit ~/.bashrc file.  
    `` ~$  vi ~/.bashrc ``  
    add the following:  
    ``export JAVA_HOME=/usr/lib/jvm/java-6-sun``
    - `` ~$  echo $JAVA_HOME ``  
    You should see this:  
    ``/usr/lib/jvm/java-6-sun``
    - Add the Canonical Partner Repository to your apt repositories:    
    `~$ sudo add-apt-repository "deb http://archive.canonical.com/ lucid partner"`    
    Or edit `~$  vi  /etc/apt/sources.list`

    - Update the source list   
    `sudo apt-get update`  
    - Install the build tools  
    `~$ sudo apt-get install build-essential sun-java6-jdk sun-java6-plugin`    
    `~$ sudo update-java-alternatives -s java-6-sun`  

* git  
    - `~$ sudo apt-get -y install git-core gitosis`  
    - As of June 2011 download [Git release is v1.7.5.4]

* Swig, Python and ssh

    - run `` ~$  sudo apt-get install swig ``
    - run `` ~$  sudo apt-get install python-dev ``
    - run `` ~$  sudo apt-get install openssh-server openssh-client ``

* Hadoop [Hadoop setup](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/)

## Mesos setup:
* Downloading Mesos:  
    `` ~$  git clone git://github.com/mesos/mesos ``  

* Building Mesos:  
    - run `` ~$  cd mesos``  
    - edit ` ~$  vi configure.template.ubuntu-lucid-64`  
    Change the following line to this:  
    ``--with-java-home=/usr/lib/jvm/java-6-sun``  
    So, `--with-java-home` option set to whatever JAVA_HOME points to.
    - run `` ~$   ~/mesos$ ./configure.template.ubuntu-lucid-64``  
```
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking target system type... x86_64-unknown-linux-gnu
===========================================================
Setting up build environment for x86_64 linux-gnu
===========================================================
running python2.6 to find compiler flags for creating the Mesos Python library...
running python2.6 to find compiler flags for embedding it...
checking for g++... g++
checking for C++ compiler default output file name... a.out

    [ ... trimmed ... ]
```
    - run `` ~/mesos$  make ``
```
make -C third_party/libprocess
make[1]: Entering directory `/home/hadoop/mesos/third_party/libprocess'
make -C third_party/glog-0.3.1
make[2]: Entering directory `/home/hadoop/mesos/third_party/libprocess/third_party/glog-0.3.1'
/bin/bash ./libtool --tag=CXX   --mode=compile g++ -DHAVE_CONFIG_H -I. -I./src  -I./src    -Wall -Wwrite-strings -Woverloaded-virtual -Wno-sign-compare  -DNO_FRAME_POINTER -DNDEBUG -O2 -fno-strict-aliasing -fPIC  -MT libglog_la-logging.lo -MD -MP -MF .deps/libglog_la-logging.Tpo -c -o libglog_la-logging.lo `test -f 'src/logging.cc' || echo './'`src/logging.cc
  
    [ ... trimmed ... ]
```

## Testing the Mesos
* run ` ~/mesos$ bin/tests/all-tests `
```
~/mesos$ bin/tests/all-tests 
[==========] Running 61 tests from 6 test cases.
[----------] Global test environment set-up.
[----------] 18 tests from MasterTest
[ RUN      ] MasterTest.ResourceOfferWithMultipleSlaves
[       OK ] MasterTest.ResourceOfferWithMultipleSlaves (33 ms)
[ RUN      ] MasterTest.ResourcesReofferedAfterReject
[       OK ] MasterTest.ResourcesReofferedAfterReject (3 ms)

[ ... trimmed ... ]

[ RUN      ] MasterTest.MultipleExecutors
[       OK ] MasterTest.MultipleExecutors (2 ms)
[----------] 18 tests from MasterTest (38 ms total)

[----------] Global test environment tear-down
[==========] 61 tests from 6 test cases ran. (17633 ms total)
[  PASSED  ] 61 tests. 
  YOU HAVE 3 DISABLED TESTS 

```

**Congratulation! You have mesos running on your Ubuntu Linux!**

## Setup mesos cluster
1. Start master: ` ~/mesos$ bin/mesos-master `
```
 ~/mesos/bin$ ./mesos-master
I0604 15:47:56.499007 1885306016 logging.cpp:40] Logging to /Users/billz/mesos/logs
I0604 15:47:56.522259 1885306016 main.cpp:75] Build: 2011-06-04 14:44:57 by billz
I0604 15:47:56.522300 1885306016 main.cpp:76] Starting Mesos master
I0604 15:47:56.522532 1885306016 webui.cpp:64] Starting master web UI on port 8080
I0604 15:47:56.522539 7163904 master.cpp:389] Master started at mesos://master@10.1.1.1:5050
I0604 15:47:56.522676 7163904 master.cpp:404] Master ID: 201106041547-0
I0604 15:47:56.522743 19939328 webui.cpp:32] Web UI thread started

[ ... trimmed ... ]
```
2. Take note of the master URL `mesos://master@10.1.1.1:5050`
3. Start slave: ` ~/mesos$ bin/mesos-slave --url=mesos://master@10.1.1.1:5050`
4. View the master's web UI at `http://10.1.1.1:8080` (here assuming this computer has IP address = 10.1.1.1).
5. Run the test framework: `~/mesos$ bin/examples/cpp-test-framework mesos://master@10.1.1.1:5050`
```
Registered!
.Starting task 0 on ubuntu.eecs.berkeley.edu
Task 0 is in state 1
Task 0 is in state 2
.Starting task 1 on ubuntu.eecs.berkeley.edu
Task 1 is in state 1
Task 1 is in state 2
.Starting task 2 on ubuntu.eecs.berkeley.edu
Task 2 is in state 1
Task 2 is in state 2
.Starting task 3 on ubuntu.eecs.berkeley.edu
Task 3 is in state 1
Task 3 is in state 2
.Starting task 4 on ubuntu.eecs.berkeley.edu
Task 4 is in state 1
Task 4 is in state 2
```

## [Running Hadoop on Mesos](https://github.com/mesos/mesos/wiki/Running-Hadoop-on-Mesos)  

We have ported version 0.20.2 of Hadoop to run on Mesos. Most of the Mesos port is implemented by a pluggable Hadoop scheduler, which communicates with Mesos to receive nodes to launch tasks on. However, a few small additions to Hadoop's internal APIs are also required.

The ported version of Hadoop is included in the Mesos project under `frameworks/hadoop-0.20.2`. However, if you want to patch your own version of Hadoop to add Mesos support, you can also use the patch located at `frameworks/hadoop-0.20.2/hadoop-mesos.patch`. This patch should apply on any 0.20.* version of Hadoop, and is also likely to work on Hadoop distributions derived from 0.20, such as Cloudera's or Yahoo!'s.

To run Hadoop on Mesos, follow these steps:

1. Setting up the environment:  
    - Create/Edit ~/.bashrc file.  
    `` ~$  vi ~/.bashrc ``  
    add the following:  
```
    # Set Hadoop-related environment variables  
    export HADOOP_HOME=/usr/local/hadoop  

    # Add Hadoop bin/ directory to PATH  
    export PATH=$PATH:$HADOOP_HOME/bin  

    # Set where you installed the mesos. For me is /home/hadoop/mesos. hadoop is my username.  
    export MESOS_HOME=/home/hadoop/mesos
```
    - Go to hadoop directory that come with mesos's directory:  
    - `cd ~/mesos/frameworks/hadoop-0.20.2/conf`  
    - Edit hadoop-env.sh file.  
    add the following:  
```
# The java implementation to use.  Required.
export JAVA_HOME=/usr/lib/jvm/java-6-sun

# The mesos use this.
export MESOS_HOME=/home/hadoop/mesos

# Extra Java runtime options.  Empty by default. This disable IPv6.
export HADOOP_OPTS=-Djava.net.preferIPv4Stack=true
```

2. Hadoop configuration:  
    - In core-site.xml file add the following:  
```
<!-- In: conf/core-site.xml -->
<property>
  <name>hadoop.tmp.dir</name>
  <value>/app/hadoop/tmp</value>
  <description>A base for other temporary directories.</description>
</property>

<property>
  <name>fs.default.name</name>
  <value>hdfs://localhost:54310</value>
  <description>The name of the default file system.  A URI whose
  scheme and authority determine the FileSystem implementation.  The
  uri's scheme determines the config property (fs.SCHEME.impl) naming
  the FileSystem implementation class.  The uri's authority is used to
  determine the host, port, etc. for a filesystem.</description>
</property>
```
    - In hdfs-site.xml file add the following:  
```
<!-- In: conf/hdfs-site.xml -->
<property>
  <name>dfs.replication</name>
  <value>1</value>
  <description>Default block replication.
  The actual number of replications can be specified when the file is created.
  The default is used if replication is not specified in create time.
  </description>
</property>
```
    - In mapred-site.xml file add the following:  
```
<!-- In: conf/mapred-site.xml -->
<property>
  <name>mapred.job.tracker</name>
  <value>localhost:9001</value>
  <description>The host and port that the MapReduce job tracker runs
  at.  If "local", then jobs are run in-process as a single map
  and reduce task.
  </description>
</property>

<property>
  <name>mapred.jobtracker.taskScheduler</name>
  <value>org.apache.hadoop.mapred.MesosScheduler</value>
</property>
<property>
  <name>mapred.mesos.master</name>
  <value>mesos://master@10.1.1.1:5050</value> <!-- Here we are assuming your host IP address is 10.1.1.1 -->
</property>

```

</li>
<li> Launch a JobTracker with <code>bin/hadoop jobtracker</code> (<i>do not</i> use <code>bin/start-mapred.sh</code>). The JobTracker will then launch TaskTrackers on Mesos when jobs are submitted.</li>
<li> Submit jobs to your JobTracker as usual.</li>
</ol>







## Need more help?   
* [Use our mailing lists](http://incubator.apache.org/projects/mesos.html)
* mesos-dev@incubator.apache.org

## Wants to contribute?
* [Contributing to Open Source Projects HOWTO](http://www.kegel.com/academy/opensource.html)
* [Submit a Bug](https://issues.apache.org/jira/browse/MESOS)