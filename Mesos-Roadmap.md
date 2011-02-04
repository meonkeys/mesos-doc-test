The Mesos development roadmap can be categorized into 3 main areas:

1. Building a community and promoting adoption
1. Core Mesos development
1. Mesos Application development

## Growing a development community and promoting adoption
1. Migrate into Apache Incubator (see the [[incubator proposal|http://wiki.apache.org/incubator/MesosProposal]])
    1. We will be migrating away from github issues and onto JIRA once we are in the incubator
    1. We are looking to grow an open-source community of developers around Mesos

## Core Mesos development (Cluster OS Kernel, i.e. scheduling, resource management, etc.)
1. More advanced allocation modules that implement the following functionality
    1. Resource revocation
    1. Resource inheritance, hierarchical scheduling
    1. A Mesos Service Level Objective
    1. Scheduling based on history
1. Migrate to ProtocolBuffers as the serialization format (under development at Twitter)
1. More advanced User Interface and [[Event History]] (i.e. logging) - See [[issue #143|https://github.com/mesos/mesos/issuesearch?state=open&q=event#issue/143]] for more details.

## Mesos application development

Technically speaking, a mesos application is defined as a mesos scheduler plus a mesos executor. Practically speaking, applications can serve many purposes. These can be broken down into a few categories.

### Applications that provide cluster OS functionality (e.g. storage, synchronization, naming, etc.)

The core of Mesos has been designed using the same philosophy behind traditional [[Microkernel operating systems|http://en.wikipedia.org/wiki/Microkernel]]. This means that the core of Mesos (the kernel, if you will) provides a minimal set of low-level abstractions for cluster resources (see [[Mesos Architecture]] for an introduction to the resource offer abstraction). This, in turn means that higher level abstractions (analogous to the filesystem, memory sharing, etc. in traditional operating systems, as well as some abstractions that have no analog in traditional single node operating systems such as DNS).

those which are primarily intended to be used by other applications (as opposed to being used by human users). They can be seen as operating system modules that implement functionality in the form of services.

1. Meta Framework Development (under development at Berkeley)
    1. Allow users to submit a job (specifying their resource constraints) and have the job wait in a queue until the resources are acquired (the Application Scheduler translates those constraints into accepting or rejecting resource offers)
1. File systems, like HDFS
1. Further Hypertable integration
1. Mesos package management application (i.e. the "apt-get" of the cluster... `apt-get install hadoop-0.20.0`)
1. A machine metadata database

## <i>Mesos framework applications</i>

Framework applications are intended to present some useful 

1. Spark

----------------

In addition to a popular, (increasingly) stable, and (soon) production system, Mesos is also a research project that is pushing the borderers of distributed system research! Check out <a href="http://mesosproject.org">the Mesos research page</a> for links to Mesos research papers, experiments, and other information about Mesos research.
