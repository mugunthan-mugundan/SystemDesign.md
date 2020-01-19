[https://leetcode.com/discuss/interview-question/483959/Google-onsite-(Theoretical-System-Design)](https://leetcode.com/discuss/interview-question/483959/Google-onsite-(Theoretical-System-Design))

Suppose you have N Machines, each running M processes(each process may have its own requirements which may change anytime and is not known from before) and you have to monitor each machine health and redistribute processes in case a Machine is overloaded.  
What all parameters would you evaluate before redistributing.

System level
CPU, Memory, IO(Disk, NW)
OS level warnings and errors

For App monitor the same things, CPU, Memory , IO.
Now if you have  apps repeatedly breaching one or more of the thresholds in a machine, you have to make a decision to rebalance.

## Monitoring methodology
You can setup a window based aggregation or a point based threshold of the resources. Based on some heuristics you can decide it is time to move something from the machine. E.g. CPU threshold 90 has been breached 10 times in the last hour or CPU threshold reached 95% once, time to move some applications from this machine.

## Re balancing 
To re balance you need
 - Current machines and their free resources.
 - Current apps on the machine and their resource consumption

### Re balancing strategies
When rebalancing you have to think about throughput and fairness. There are several things you may want to consider before moving. 
There may be one application that is causing the breach. In this case you can either move the offending application to a machine that has enough resource. Or you can move the other applications and give the application that is requiring large resources to this machine. The decision has to be based on improving throughput and fairness. If the breaching application has done a lot of work and would have to restart the work, it may be better to leave the application. There. A last in first out may be also used. Keeping the breaching app on the same machine also allows the application being kicked around many machines.  Also, it matters if the resources are infinite or finite. If finite then LIFO has to be limited to x number of moves, after that we have to make sure that the app gets a chance to run. 

## Offending apps
Some apps may not be able to be scheduled because they do not fit into single machine. We may have to fail them permanently.
