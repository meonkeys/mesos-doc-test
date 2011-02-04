The Mesos development roadmap can be categorized into 4 main areas.

## Growing a development community and project organization
1. Migrate into Apache Incubator (see the [[incubator proposal|http://wiki.apache.org/incubator/MesosProposal]])
    1. We will be migrating away from github issues and onto JIRA once we are in the incubator
    1. We are looking to grow an open-source community of developers around Mesos

## Core Mesos development (Cluster OS Kernel, i.e. scheduling, resource management, etc.)
1. More advanced allocation modules
    1. Resource revocation
1. Migrate to ProtocolBuffers as the serialization format (under development at Twitter)
1. More advanced User Interface and [[Event History]] (i.e. logging) - See [[issue #143|https://github.com/mesos/mesos/issuesearch?state=open&q=event#issue/143]] for more details.

## <i>Mesos service applications</i> that implement other cluster OS functionality (e.g. storage, synchronization, naming, etc.)

Service applications are those which are primarily intended to be used by other applications (as opposed to being used by human users).

1. Meta Framework Development (under development at Berkeley)
    1. Allow users to submit a job (specifying their resource constraints) and have the job wait in a queue until the resources are acquired (the Application Scheduler translates those constraints into accepting or rejecting resource offers)
1. HDFS
1. Further Hypertable integration
1. Mesos package management application (i.e. the "apt-get" of the cluster... `apt-get install hadoop-0.20.0`)

## <i>Mesos framework applications</i>

Framework applications are 
1. Spark

----------------

In addition to a popular, (increasingly) stable, and (soon) production system, Mesos is also a research project that is pushing the borderers of distributed system research! Check out <a href="http://mesosproject.org">the Mesos research page</a> for links to Mesos research papers, experiments, and other information about Mesos research.
