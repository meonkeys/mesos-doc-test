1. Migrate into Apache Incubator (see the [[incubator proposal|http://wiki.apache.org/incubator/MesosProposal]])
    1. We will be migrating away from github issues and onto JIRA once we are in the incubator
    1. We are looking to grow an open-source community of developers around Mesos
1. Resource revocation
1. [[Event History]] - See [[issue #143|https://github.com/mesos/mesos/issuesearch?state=open&q=event#issue/143]] for more details.
1. Migrate to ProtocolBuffers as the serialization format (under development at Twitter)
1. Meta Framework Development (under development at Berkeley)
    1. Allow users to submit a job (specifying their resource constraints) and have the job wait in a queue until the resources are acquired (the Application Scheduler translates those constraints into accepting or rejecting resource offers)