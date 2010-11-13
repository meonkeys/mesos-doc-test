-> [[/images/architecture3.jpg|height=300px]] <-

The above figure shows the main components of Mesos.  Mesos consists of a <b>master</b> process that manages <b>slave</b> daemons running on each cluster node, and <b>frameworks</b> that run <b>tasks</b> on these slaves. 

The master implements fine-grained sharing across frameworks using <i>resource offers</i>. Each resource offer contains a list of free resources on multiple slaves.  The master decides <b>how many</b> resources to offer to each framework according to a given organizational policy, such as fair sharing, or strict priority. To support a diverse set of policies, the master employs a modular architecture that makes it easy to add new allocation modules via a plugin mechanism.

A framework running on top of Mesos consists of two components: a <b>scheduler</b> that registers with the master to be offered resources, and an <b>executor</b> process that is launched on slave nodes to run the framework's tasks. While the master determines how many resources are offered to each framework, the frameworks' schedulers select <b>which</b> of the offered resources to use. When a frameworks accepts offered resources, it passes to Mesos a description of the tasks it wants to run on them. In turn, Mesos launches the tasks on the corresponding slaves.

h2. Example of resource offer 

->[[/images/architecture-example.jpg|height=300px]]<-

The above figure shows an example of how a framework gets scheduled to run a task. Let's walk through the events in the picture.

1. Slave 1 reports to the master that it has 4 CPUs and 4 GB of memory free. The master then invokes the allocation policy module, which tells it that framework 1 should be offered all available resources.
2. The master sends a resource offer describing what is available on slave 1 to framework 1.  
3. The framework's scheduler replies to the master with information about two tasks to run on the slave, using < 2 CPUs, 1 GB RAM> for the first task, and <1 CPUs, 2 GB RAM> for the second task. 
4. Finally, the master sends the tasks to the slave, which allocates appropriate resources to the framework's executor, which in turn launches the two tasks (depicted with dotted-line borders in the figure). Because 1 CPU and 1 GB of RAM are still unallocated, the allocation module may now offer them to framework 2.

In addition, this resource offer process repeats when tasks finish and new resources become free.

While the thin interface provided by Mesos allows it to scale and allows the frameworks to evolve independently, one question remains: how can the constraints of a framework be satisfied without Mesos knowing about these constraints? For example, how can a framework achieve data locality without Mesos knowing which nodes store the data required by the framework? Mesos answers these questions by simply giving frameworks the ability to <b>reject</b> offers. A framework will reject the offers that do not satisfy its constraints and accept the ones that do.  In particular, we have found that a simple policy called delay scheduling \cite{delay-scheduling}, in which frameworks wait for a limited time to acquire nodes storing the input data, yields nearly optimal data locality.

You can also read much more about the Mesos architecture in this this [[technical paper|http://mesos.berkeley.edu/mesos_tech_report.pdf]].
