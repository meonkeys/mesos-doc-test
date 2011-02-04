There are two main ways to get a Mesos cluster running on EC2 quickly and easily. One way is via the Mesos [[EC2-Scripts]]. The main guts of the EC2 Scripts are the python program in <mesos-download-root-dir>/src/ec2/mesos_ec2.py which will start up a number of Amazon EC2 Instances (these images already contain a copy of mesos at /root/mesos), and then SSH into those machines, set up the configuration files on the slaves to talk to the master, and also set up HDFS, NFS, and more on those nodes!

The other main way to launch a Mesos cluster on EC2 is using the Mesos "Ready-to-go" AMI. We have set up this AMI to make taking Mesos for a spin as easy as launching a bunch of new instances in EC2. That is, we have pre-packaged an AMIs with /etc/init.d/mesos-master and /etc/init.d/mesos-slave scripts that make running Mesos on those machines super easy!

The most recent version of AMI (which is updated regularly, so check back often) is: ami-b87383d1 

## Prerequisite: Have a functional EC2 account

Here are some high level instructions for getting started with Amazon EC2:

1. Set up you Amazon EC2 user account
1. Download the [[Amazon EC2 API tools|http://www.amazon.com/gp/redirect.html/ref=aws_rc_ec2tools?location=http://s3.amazonaws.com/ec2-downloads/ec2-api-tools.zip&token=A80325AA4DAB186C80828ED5138633E3F49160D9]] or install [[Elasticfox|http://aws.amazon.com/developertools/609?_encoding=UTF8&jiveRedirect=1]]
1. Set up your EC2 credentials and try out the tools by running: `ec2-describe-instances`
1. Make sure your security groups 

## How to use the "ready to go" AMI to run a Mesos Master node

1. ec2-run-instances <AMI-IDNUM> #see above for most recent AMI-IDNUM to use`
1. SSH into that machine as root and run: `service mesos-master start`

## How to use the "ready to go" AMI to run a Mesos Slave node

1. Run the slave AMI and pass in user data containing the url of a running Mesos master: `ec2-run-instances <AMI-IDNUM> -d url=1@<master-ip or hostname>:<master port> #see above for most recent AMI-IDNUM to use`
