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
   -`` ~$  cd mesos``  
   -`` ~$   ~/mesos$ ./configure.template.macosx``  
   -