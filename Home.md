# Overview of Mesos

Mesos is a cluster management platform that provides resource sharing and isolation across distributed applications. For example, [Hadoop MapReduce](http://hadoop.apache.org), [MPI](http://www.mcs.anl.gov/research/projects/mpich2/), [Hypertable](http://hypertable.org), and [Spark](http://github.com/mesos/spark/wiki) (a new MapReduce-like framework from the Mesos team that supports low-latency interactive and iterative jobs) can run on Mesos.

#### Mesos Use-cases
* Run Hadoop, MPI, Spark and other cluster applications on a dynamically shared pool of machines
* Run **multiple instances of Hadoop** on the same cluster, e.g. separate instances for production and experimental jobs, or even multiple versions of Hadoop
* Build new cluster applications without having to reinvent low-level facilities for running tasks on different nodes, monitoring them, etc., and have your applications coexist with existing ones

#### Features of Mesos
* Master fault tolerance using ZooKeeper
* Isolation between tasks using Linux Containers
* Memory and CPU aware allocation
* Efficient fine-grained resource sharing abstraction (<i>resource offers</i>) that allows applications to achieve app-specific scheduling goals (e.g. Hadoop optimizes for data locality)
<br/>
Visit [mesosproject.org](http://mesosproject.org) for more details about Mesos.

_**Please note that Mesos is still in beta. Though the current version is in use in production at Twitter, it may have some stability issues in certain environments.**_

#### What you'll find on this page
1. Quick-start Guides
1. System Requirements
1. Downloading Mesos
1. Building Mesos
1. Testing the Build
1. Deploying to a Cluster
1. Where to Go From Here
<br/>

# Quick-Start Guides

* [[Running Mesos On Ubuntu Linux (Single Node Cluster)]]
* [[Running Mesos On Mac OS X Snow Leopard (Single Node Cluster)]]
* [[Running Mesos On Mac OS X Lion (Single Node Cluster)]]

# System Requirements

Mesos runs on [Linux](https://github.com/mesos/mesos/wiki/Running-Mesos-On-Ubuntu-Linux-(Single-Node-Cluster)) and [Mac OS X](https://github.com/mesos/mesos/wiki/Running-Mesos-On-Mac-OS-X-(Single-Node-Cluster)), and has previously also been tested on OpenSolaris. You will need the following packages to run it:

* g++ 4.1 or higher.
* Python 2.6 (for the Mesos web UI). On Mac OS X 10.6 or earlier, get Python from [MacPorts](http://www.macports.org/) via `port install python26`, because OS X's version of Python is not 64-bit.
* Java JDK 1.6 or higher. Mac OS X 10.6 users will need to install the JDK from http://connect.apple.com/ and set `JAVA_HEADERS=/System/Library/Frameworks/JavaVM.framework/Versions/A/Headers`.
 

# Downloading Mesos

We are actively working on our first official Apache release. Currently, you can obtain Mesos (the current development HEAD) by checking it out from either the Apache SVN or Apache Git repository (the git repo is a mirror of the SVN repo)
* For SVN, use: `svn co https://svn.apache.org/repos/asf/incubator/mesos/trunk mesos-trunk`
* For git, use: `git clone git://git.apache.org/mesos.git`

NOTES: The Apache SVN repository is the definitive spot for the source code now. For alpha version 0.4 and before, you can still download tagged "alpha releases" from github via the [[tags page|https://github.com/mesos/mesos/tags]]. However, we do not recommend using the Github repository hosted under the mesos user (e.g. `git://github.com/mesos/mesos.git`) any longer. Though it is a little confusing, Apache also maintains a github clone of the SVN repository at `https://github.com/apache/mesos`, which should be fine to use as an alternative to the SVN and git apache.org repositories listed above (though the committers don't recommend it).

# Building Mesos

Mesos uses the standard GNU build tools. 

NOTE: Until our first Apache release, we <i>strongly</i> recommend that you continue to keep the version of Mesos that you are using up to date with the SVN trunk. <i><b>If you are using an old version of Mesos</b></i>, you may need to use [Old Mesos Build Instructions].

We migrated the Mesos build system on Jan 19th 2012 to using Autotools. If you are using an up to date version of Mesos trunk (checked out from SVN), follow these instructions:

1. run `./bootstrap`

2. run `./configure --with-webui --with-included-zookeeper` (these flags are what we recommend, you may want to exclude these flags or use others, see below).

The configure script itself accepts the following arguments to enable various options:

* `--with-python-headers=DIR`: Find Python header files in `DIR` (to turn on Python support). Recommended.
* `--with-webui`: Enable the Mesos web UI (which requires Python 2.6). Recommended.
* `--with-java-home=DIR`: Enable Java application/framework support with a given installation of Java. Required for Hadoop and Spark.
* `--with-java-headers=DIR`: Find Java header files (necessary for newer versions of OS X Snow Leopard).
* `--with-zookeeper=DIR` or `--with-included-zookeeper`: Enable master fault-tolerance using an existing ZooKeeper installation or the version of ZooKeeper bundled with Mesos. For details, see [[using ZooKeeper]].

3. run `make`

### NOTES:
* In the SVN trunk branch since Jan 19 2012 (when Mesos switched fully to the GNU Autotools build system), the build process attempts to guess where your Java include directory is but if you have set the $JAVA_HOME environment variable, it will use $JAVA_HOME/include, which may not be correct for your machine (in which case you will see an error such as: "configure: error: failed to build against JDK (using libtool)"). If this is the case, we suggest you unset the JAVA_HOME environment variable.
* (NOT SURE IF THIS IS STILL RELEVANT) On 32-bit platforms, you should set `CXXFLAGS="-march=i486"` when running `configure` to ensure certain atomic instructions can be used.
* (NOT SURE IF THIS IS STILL RELEVANT) `configure` prints a warning at the end about "unrecognized options: --with-java-home, ...". This comes from one of the nested `configure` scripts that we call, so it doesn't mean that your options were ignored.

# Testing the Build

After you build Mesos, you can run its unit tests using the `mesos/bin/tests/all-tests` program located in `bin/tests`. Note that a few tests for specific platforms are disabled by default. You can run `all-tests` with `--help` to view its options.

You can also set up a small Mesos cluster and run a job on it as follows:

1. Go into the directory where you built Mesos.
1. Type `bin/mesos-master` to launch the master.
1. Take note of the master URL that is printed to stdout, which will look something like <code>mesos://master@192.168.0.1:5050</code> (in this example the IP address of master is 192.168.0.1).
1. URL of master: <code>mesos://master@192.168.0.1:5050</code>
1. View the master's web UI at `http://[hostname of master]:8080`.
1. Launch a slave by typing <code>bin/mesos-slave --master=mesos://master@192.168.0.1:5050</code>. The slave will show up on the master's web UI if you refresh it.
1. Run the C++ test framework (a sample that just runs five tasks on the cluster) using <code>bin/examples/cpp-test-framework mesos://master@192.168.0.1:5050</code>. It should successfully exit after running five tasks. You can also try the example python or Java frameworks.

# Deploying to a Cluster

To run Mesos on more than one machine, you have multiple options:

* To launch a cluster on a private cluster that you own, use Mesos's [[deploy scripts]]
* To launch a cluster on Amazon EC2, you can use the Mesos [[EC2 scripts]]

# Where to Go From Here

* [[Mesos architecture]] -- an overview of Mesos concepts.
* [[Mesos Developers guide]] -- resources for developers contributing to Mesos. Style guides, tips for working with SVN/git, and more!
* [[App/Framework development guide]] -- learn how to build applications on top of Mesos.
* [[Configuring Mesos|Configuration]] -- a guide to the various settings that can be passed to Mesos daemons.
* Mesos system feature guides:
    * [[Deploy scripts]] for launching a Mesos cluster on a set of machines.
    * [[EC2 scripts]] for launching a Mesos cluster on Amazon EC2.
    * [[Logging and Debugging]] -- viewing Mesos and framework logs.
    * [[Using ZooKeeper]] for master fault-tolerance.
    * [[Using Linux Containers]] for resource isolation on slaves.
* Guide to running existing frameworks:
    * [[Running Spark on Mesos|https://github.com/mesos/spark/wiki]]
    * [[Using the Mesos Submit tool]]
    * [[Using Mesos with Hypertable on EC2|http://code.google.com/p/hypertable/wiki/Mesos]] (external link) - Hypertable is a distributed database platform.
    * [[Running Hadoop on Mesos]]
    * [[Running a web application farm on Mesos]]
    * [[Running Torque or MPI on Mesos]]
* [[Powered-by|Powered-by Mesos]] -- Projects that are using Mesos!
* [[Mesos code internals overview|Mesos Code Internals]] -- an overview of the codebase and internal organization.
* [[Mesos development roadmap|Mesos Roadmap]]
* [[The official Mesos website|http://mesosproject.org]]
