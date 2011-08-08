## Mesos VMWare Image with Hadoop pre-installed

***
Last Updated: August 2011

[DOWNLOAD](http://amplab.cs.berkeley.edu/downloads/mesos/mesos-demo.tar.bz2)

```
Note: This VM download is approximately 1.7GB.  
Feel Free to mirror internally or externally to minimize bandwidth usage.
The Uncompressed version of VM required 6GB of disk space.
```

To make it easy for you to get started with Apache Mesos, we created a virtual machine with everything you need. our VM runs Ubuntu 10.04 LTS - Long-term support 64-bit (Lucid Lynx) and Mesos with Apache Hadoop 0.20.2.

To launch the VMWare image, you will either need [VMware Player for Windows and Linux](http://www.vmware.com/go/downloadplayer/), or [VMware Fusion for Mac](http://www.vmware.com/products/fusion/). (Note that VMware Fusion only works on Intel architectures, so older Macs with PowerPC processors cannot run the training VM.)

Once you launch the VM, log in with the following account details:  

  - username: hadoop  
  - password: hadoop

The **hadoop** account has *sudo* privileges in the VM.

### Testing the Mesos  
* run ` ~/mesos$ bin/tests/all-tests `  
<code> 
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
  
</code>  

Click the Firefox Web Browser on the Panel to view Mesos's documentation.
The browser also provides the following bookmarks:   

   *  [http://localhost:50030](http://localhost:50030) - web UI for MapReduce job tracker(s)  
   *  [http://localhost:50060](http://localhost:50060) - web UI for task tracker(s)  
   *  [http://localhost:50070](http://localhost:50070) - web UI for HDFS name node(s)  
   *  [http://localhost:8080](http://localhost:8080) - web UI for Mesos master  