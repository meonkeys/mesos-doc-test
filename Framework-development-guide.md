# Create your Framework Scheduler

See one of the example framework schedulers to get an idea of what the framework scheduler might look like.

You can write a framework scheduler in C, C++, Java (, Scala), or Python. Your framework scheduler should inherit from Scheduler, and it should create a SchedulerDriver and then call SchedulerDriver.run();

# Create your Framework Executor

# Install your Framework

You can use the $MESOS_HOME environment variable inside of your executor to determine where mesos is running from.