h1. Overview of Mesos

NOTE THAT MESOS IS NOW AN APACHE PROJECT and this mesos code on github is old and no longer maintained.  Use the svn to see the latest changes to Mesos: http://www.mesosproject.org/download.html


Mesos is a cluster management platform that provides resource sharing and isolation across distributed applications. For example, "Hadoop MapReduce":http://hadoop.apache.org, "MPI":http://www.mcs.anl.gov/research/projects/mpich2/, "Hypertable":http://hypertable.org, and "Spark":http://github.com/mesos/spark/wiki (a new MapReduce-like framework from the Mesos team that supports low-latency interactive and iterative jobs) can run on Mesos.

h4. Mesos Use-cases
* Run Hadoop, MPI, Spark and other cluster applications on a dynamically shared pool of machines
* Run **multiple instances of Hadoop** on the same cluster, e.g. separate instances for production and experimental jobs, or even multiple versions of Hadoop
* Build new cluster applications without having to reinvent low-level facilities for running tasks on different nodes, monitoring them, etc., and have your applications coexist with existing ones

h4. Features of Mesos
* Master fault tolerance using ZooKeeper
* Isolation between tasks using Linux Containers
* Memory and CPU aware allocation
* Efficient fine-grained resource sharing abstraction (<i>resource offers</i>) that allows applications to achieve app-specific scheduling goals (e.g. Hadoop optimizes for data locality)
<br/>
Visit "mesosproject.org":http://mesosproject.org for more details about Mesos.

_*Please note that Mesos is still in alpha. The current version is intended for early testing.*_

h4. What you'll find on this page</u></b>
# System Requirements
# Downloading Mesos
# Building Mesos
# Testing the Build
# Deploying to a Cluster
# Where to Go From Here
<br>

h1. Quick-Start Guides

* [[Running Mesos On Mac OS X Snow Leopard (Single Node Cluster)]]
* [[Running Mesos On Mac OS X Lion (Single Node Cluster)]]
* [[Running Mesos On Ubuntu Linux (Single Node Cluster)]]


h1. System Requirements

Mesos runs on "Linux":https://github.com/mesos/mesos/wiki/Running-Mesos-On-Ubuntu-Linux-(Single-Node-Cluster) and "Mac OS X":https://github.com/mesos/mesos/wiki/Running-Mesos-On-Mac-OS-X-(Single-Node-Cluster), and has previously also been tested on OpenSolaris. You will need the following packages to run it:

* g++ 4.1 or higher
* SWIG 1.3.40 or higher (If you install swig on Mac OS X via "MacPorts":http://www.macports.org/, run @sudo port install swig swig-java swig-python@)
* Python 2.6 (for the Mesos web UI)
* Java 1.6 or higher (to run Java apps/frameworks including Hadoop and "Spark":http://github.com/mesos/spark). Mac OS X 10.6 users will need to install the JDK from http://connect.apple.com/ and set @JAVA_HEADERS=/System/Library/Frameworks/JavaVM.framework/Versions/A/Headers@.
 

h1. Downloading Mesos

We are actively working on our first official Apache release. Currently, you can obtain Mesos (the current development HEAD) by checking it out from either the Apache SVN or Apache Git repository (the git repo is a mirror of the SVN repo)
* For SVN, use: @svn co https://svn.apache.org/repos/asf/incubator/mesos/trunk mesos-trunk@
* For git, use: @git clone git://git.apache.org/mesos.git@

NOTES: The Apache SVN repository is the definitive spot for the source code now. For alpha version 0.4 and before, you can still download tagged "alpha releases" from github via the [[tags page|https://github.com/mesos/mesos/tags]]. However, we do not recommend using the Github repository hosted under the mesos user (e.g. @git://github.com/mesos/mesos.git@) any longer. Though it is a little confusing, Apache also maintains a github clone of the SVN repository at @https://github.com/apache/mesos@, which should be fine to use as an alternative to the SVN and git apache.org repositories listed above (though the committers don't recommend it).

h1. Building Mesos

Mesos uses the standard GNU build tools. Depending on whether you are running on OS X or Ubuntu, you may find it convenient to use one of the configure.template scripts in the root directory that will call the more general @configure@ script and pass it appropriate arguments. 

These configure template scripts try to guess the right Java and Python paths for Mac OS X and several Linux distributions. However, they assume that you have installed the necessary packages (e.g. @python-dev@ and a JDK). You should double check the configure template script you use, for example, to make sure that if you have installed Java Sun 1.6 your configure template script is not setting java_home to be for openjdk.

The configure script itself accepts the following arguments to enable various options:
* @--with-python-headers=DIR@: Find Python header files in @DIR@ (to turn on Python support). Recommended.
* @--with-webui@: Enable the Mesos web UI (which requires Python 2.6). Recommended.
* @--with-java-home=DIR@: Enable Java application/framework support with a given installation of Java. Required for Hadoop and Spark.
* @--with-java-headers=DIR@: Find Java header files (necessary for newer versions of OS X Snow Leopard).
* @--with-zookeeper=DIR@ or @--with-included-zookeeper@: Enable master fault-tolerance using an existing ZooKeeper installation or the version of ZooKeeper bundled with Mesos. For details, see [[using ZooKeeper]].

After you have run @configure@ (either directly or with a configure template script), you can build Mesos with @make@.

*Notes:*
* On 32-bit platforms, you should set @CXXFLAGS="-march=i486"@ when running @configure@ to ensure certain atomic instructions can be used.
* @configure@ prints a warning at the end about "unrecognized options: --with-java-home, ...". This comes from one of the nested @configure@ scripts that we call, so it doesn't mean that your options were ignored.

h1. Testing the Build

After you build Mesos, you can run its unit tests using the @mesos/bin/tests/all-tests@ program located in @bin/tests@. Note that a few tests for specific platforms are disabled by default. You can run @all-tests@ with @--help@ to view its options.

You can also set up a small Mesos cluster and run a job on it as follows:
# Go into the directory where you built Mesos.
# Type @bin/mesos-master@ to launch the master.
# Take note of the master URL that is printed to stdout, which will look something like <code>mesos://master@192.168.0.1:5050</code> (here assuming the IP address of master is 192.168.0.1. Please use IP address of the host instead of localhost or 127.0.0.1.)
# URL of master: <code>mesos://master@192.168.0.1:5050</code>
# View the master's web UI at @http://[hostname of master]:8080@.
# Launch a slave by typing <code>bin/mesos-slave --master=mesos://master@192.168.0.1:5050</code>. The slave will show up on the web UI if you refresh it.
# Run the C++ test framework (a sample that just runs five tasks on the cluster) using <code>bin/examples/cpp-test-framework mesos://master@192.168.0.1:5050</code>. It should successfully exit after running five tasks.

h1. Deploying to a Cluster

Once you have run the basic tests, you can use Mesos's [[deploy scripts]] to launch a cluster on a private cluster that you own. To launch a cluster on Amazon EC2, you can use the Mesos [[EC2 scripts]] or the [[Mesos "Ready-to-go" AMI|Mesos ready to go AMI]].

h1. Where to Go From Here

* [[Mesos architecture]] -- an overview of Mesos concepts.
* [[Mesos Developers guide]] -- resources for developers contributing to Mesos. Style guides, tips for working with SVN/git, and more!
* [[App/Framework development guide]] -- learn how to build applications on top of Mesos.
* [[Configuring Mesos|Configuration]] -- a guide to the various settings that can be passed to Mesos daemons.
* Mesos system feature guides:
** [[Deploy scripts]] for launching a Mesos cluster on a set of machines.
** [[EC2 scripts]] for launching a Mesos cluster on Amazon EC2.
** [[Mesos "Ready-to-go" AMI|Mesos ready to go AMI]] -- bring up EC2 instances that already have a Mesos master or slave daemon installed and running.
** [[Logging and Debugging]] -- viewing Mesos and framework logs.
** [[Using ZooKeeper]] for master fault-tolerance.
** [[Using Linux Containers]] for resource isolation on slaves.
* Guide to running existing frameworks:
** [[Running Spark on Mesos|https://github.com/mesos/spark/wiki]]
** [[Using the Mesos Submit tool]]
** [[Using Mesos with Hypertable on EC2|http://code.google.com/p/hypertable/wiki/Mesos]] (external link) - Hypertable is a distributed database platform.
** [[Running Hadoop on Mesos]]
** [[Running a web application farm on Mesos]]
** [[Running Torque or MPI on Mesos]]
* [[Powered-by|Powered-by Mesos]] -- Projects that are using Mesos!
* [[Mesos code internals overview|Mesos Code Internals]] -- an overview of the codebase and internal organization.
* [[Mesos development roadmap|Mesos Roadmap]]
* [[The official Mesos website|http://mesosproject.org]]
