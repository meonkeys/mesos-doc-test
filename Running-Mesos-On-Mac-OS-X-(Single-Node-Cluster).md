## Running Mesos On Mac OS X (Single Node Cluster)  
This is step-by-step guide on setting up Mesos on a single node, and running hadoop on top of Mesos.

## Prerequisites:
* Java
    For Mac OS X 10.6 (Snow Leopard):  
    - Start the Terminal app.  
    - Create/Edit ~/.bash_profile file.  
    - `` ~$  vi ~/.bash_profile ``  
    add the following:  
    ``export JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home``
    - `` ~$  echo $JAVA_HOME ``  
    You should see this:  
    ``/System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home``

* git  
    - Download and install the lastest version of [git](http://git-scm.com/) for Mac OS X
    - As of June 2011 download [1.7.5.4 - OS X - Leopard - x86_64](http://code.google.com/p/git-osx-installer/downloads/detail?name=git-1.7.5.4-x86_64-leopard.dmg&can=3&q=) 

* Install the latest [Xcode](developer.apple.com) 

* macport
    - Download and install [Macport](http://www.macports.org/install.php) 
    - As of June 2011 download [Macport for Snow Leopard](http://distfiles.macports.org/MacPorts/MacPorts-1.9.2-10.6-SnowLeopard.dmg)
    - run `` ~$  sudo port -d selfupdate ``
    - run `` ~$  sudo port upgrade outdated ``
    - run `` ~$  sudo port install swig ``
    - run `` ~$  sudo port install swig-java ``
    - run `` ~$  sudo port install swig-python ``

* Hadoop [Hadoop setup](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/)

## Mesos setup:
* Downloading Mesos:  
    `` ~$  git clone git://github.com/mesos/mesos ``  

* Building Mesos:  
    - run `` ~$  cd mesos``  
    - run `` ~$   ~/mesos$ ./configure.template.macosx``  
```
  checking build system type... i386-apple-darwin10.7.0
	checking host system type... i386-apple-darwin10.7.0
	checking target system type... i386-apple-darwin10.7.0
	===========================================================
	Setting up build environment for i386 darwin10.7.0
	===========================================================
	running python2.6 to find compiler flags for creating the Mesos Python library...
	running python2.6 to find compiler flags for embedding it...
	checking for g++... g++

	[ ... trimmed ... ]
```
    - run `` ~/mesos$  make ``
```
make -C third_party/libprocess
make -C third_party/glog-0.3.1
/bin/sh ./libtool --tag=CXX   --mode=compile g++ -DHAVE_CONFIG_H -I. -I./src  -I./src    -Wall -Wwrite-strings -Woverloaded-virtual -Wno-sign-compare  -DNO_FRAME_POINTER -DNDEBUG -O2 -fno-strict-aliasing -fPIC  -D_XOPEN_SOURCE -MT libglog_la-logging.lo -MD -MP -MF .deps/libglog_la-logging.Tpo -c -o libglog_la-logging.lo `test -f 'src/logging.cc' || echo './'`src/logging.cc

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
[ RUN      ] MasterTest.ResourcesReofferedAfterBadResponse
[       OK ] MasterTest.ResourcesReofferedAfterBadResponse (2 ms)
[ RUN      ] MasterTest.SlaveLost
[       OK ] MasterTest.SlaveLost (2 ms)
[ ... trimmed ... ]
```

**Congratulation! You have mesos running on your Mac OS X!**

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
.Starting task 0 on mac.eecs.berkeley.edu
Task 0 is in state 1
Task 0 is in state 2
.Starting task 1 on mac.eecs.berkeley.edu
Task 1 is in state 1
Task 1 is in state 2
.Starting task 2 on mac.eecs.berkeley.edu
Task 2 is in state 1
Task 2 is in state 2
.Starting task 3 on mac.eecs.berkeley.edu
Task 3 is in state 1
Task 3 is in state 2
.Starting task 4 on mac.eecs.berkeley.edu
Task 4 is in state 1
Task 4 is in state 2
```

## Running Hadoop on Mesos  [Running Hadoop on Mesos](https://github.com/mesos/mesos/wiki/Running-Hadoop-on-Mesos)

