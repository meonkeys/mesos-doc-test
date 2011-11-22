1. First, follow the instructions [here](https://github.com/mesos/mesos/wiki/Running-Mesos-On-Mac-OS-X-Snow-Leopard-(Single-Node-Cluster\)) to get a single instance of Hadoop running.

2. Next, we'll get a second instance of the same version of Hadoop that ships with Mesos running:
    - Make another copy of Hadoop:      
      `~/mesos$ cp -R frameworks/hadoop-0.20.2 ~/hadoop`
    - Modify necessary ports:     
        * In conf/mapred-site.xml.template, change the mapred.job.tracker port from 9001 to 9002.
        * In src/mapred/mapred-default.xml, change the mapred.task.tracker.http.address port and the      
          mapred.job.tracker.http.address port both to 0.
    - Build Hadoop:      
      `~/hadoop$ ant`      
      `~/hadoop$ ant compile-core jar`      
      `~/hadoop$ ant examples jar`      
    - Start up HDFS (we'll have one instance of HDFS that both instances of Hadoop access):      
      `~/mesos/frameworks/hadoop-0.20.2$ bin/hadoop namenode`      
      `~/mesos/frameworks/hadoop-0.20.2$ bin/hadoop datanode`
    - Start the jobtracker:
      `~/mesos/frameworks/hadoop-0.20.2$ bin/hadoop jobtracker`