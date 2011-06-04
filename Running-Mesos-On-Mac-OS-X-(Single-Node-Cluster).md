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

* macport
    - Download and install [Macport](http://www.macports.org/install.php) 
    - As of June 2011 download [Macport for Snow Leopard](http://distfiles.macports.org/MacPorts/MacPorts-1.9.2-10.6-SnowLeopard.dmg)
    - run `` ~$  sudo port -d selfupdate ``
    - run `` ~$  sudo port upgrade outdated ``
    - run `` ~$  sudo port install swig ``
    - run `` ~$  sudo port install swig-java ``
    - run `` ~$  sudo port install swig-python ``

* Hadoop

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