We have ported version 0.20.2 of Hadoop to run on Mesos. Most of the Mesos port is implemented by a pluggable Hadoop scheduler, which communicates with Mesos to receive nodes to launch tasks on. However, a few small additions to Hadoop's internal APIs are also required.

The ported version of Hadoop is included in the Mesos project under `frameworks/hadoop-0.20.2`. However, if you want to patch your own version of Hadoop to add Mesos support, you can also use the patch located at `frameworks/hadoop-0.20.2/hadoop-mesos.patch`. This patch should apply on any 0.20.* version of Hadoop, and is also likely to work on Hadoop distributions derived from 0.20, such as Cloudera's or Yahoo!'s.

To run Hadoop on Mesos, follow these steps:
<ol>
<li> Build Hadoop using <code>ant</code>.</li>
<li> Set up Hadoop's configuration as you would usually do with a new install of Hadoop, following the [[instructions on the Hadoop website|http://hadoop.apache.org/common/docs/r0.20.2/index.html]] (at the very least, you need to set <code>JAVA_HOME</code> in Hadoop's <code>conf/hadoop-env.sh</code> and set <code>mapred.job.tracker</code> in <code>conf/mapred-site.xml</code>).</li>
</li>
<li> Add the following parameters to Hadoop's <code>conf/mapred-site.xml</code>:
<pre>
&lt;property&gt;
  &lt;name&gt;mapred.jobtracker.taskScheduler&lt;/name&gt;
  &lt;value&gt;org.apache.hadoop.mapred.MesosScheduler&lt;/value&gt;
&lt;/property&gt;
&lt;property&gt;
  &lt;name&gt;mapred.mesos.master&lt;/name&gt;
  &lt;value&gt;[URL of Mesos master]&lt;/value&gt;
&lt;/property&gt;
</pre>
</li>
<li> Launch a JobTracker with <code>bin/hadoop jobtracker</code> (<i>do not</i> use <code>bin/start-mapred.sh</code>). The JobTracker will then launch TaskTrackers on Mesos when jobs are submitted.</li>
<li> Submit jobs to your JobTracker as usual.</li>
</ol>

Note that when you run on a cluster, Hadoop (and Mesos) should be located on the same path on all nodes.

If you wish to run multiple JobTrackers, the easiest way is to give each one a different port by using a different Hadoop `conf` directory for each one and passing the `--conf` flag to `bin/hadoop` to specify which config directory to use. You can copy Hadoop's existing `conf` directory to a new location and modify it to achieve this.

Finally, if you run your own patched version of Hadoop instead of the one included in Mesos, you will need to take an additional step before building and running Hadoop: you must set the `MESOS_HOME` environment variable to the location where Mesos is found. You need to do this both in your shell environment when you run `ant`, and in Hadoop's `hadoop-env.sh`.