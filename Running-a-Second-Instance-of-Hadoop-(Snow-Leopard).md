First, follow the instructions [here](https://github.com/mesos/mesos/wiki/Running-Mesos-On-Mac-OS-X-Snow-Leopard-(Single-Node-Cluster\)) to get a single instance of Hadoop running.

1. First, we'll get a second instance of the same version of Hadoop that ships with Mesos running:
    - Make another copy of Hadoop:      
      `~/mesos$ cp -R frameworks/hadoop-0.20.2 ~/hadoop`
    - Modify necessary ports:     
        * In 