# to demostrate schedulinjg class of kubernetes scheduling pod with different examples
1. Mannuallu Scheduling
2. Node selector
3. nodeAffinity
4. Taint and tolerations
5. Daemonsets
6. Static pods
7. Priority class
# label node:
1. kubectl label node **node-name** **<**key**=**value** to label it with desired key and value, 
2. kubectl label node **node-name** **key**- to unlabel it.
 - this labels can be put into pods by pod sec: nodeName/nodeSelector properties
 # node affinity:
 1. for node affinity, defined in pod spec, required during scheduling, ignored during execution, condition,
   - matched with node name previously set on node, with expression making it check pod spec for node KVP, duriong scheduling.
   - operators used are In, Exists, NotIn, Gt, Lt, DoesnotExists, to configure logic fo node properties on pod.
   - these operators mixec with key and values , pods could be pushed to specific set of nodes to get scheduled and executed.
   - If this specific key and value exits on pod matched with node, then this pod is prefered to be scheduled on this node.
   - kubectl label node node01 backup-node=yes
   when used Exists operator, if just key exits on node label then condition is met, for nodeaffinity.
# Taints and tolerations:
1. Ristrictings pods without proper tolerations added to their spec, wont be scheduled.
2. Taints are registred on any desired node on depending on logic of which type of pods are prefrerred and not preferred on this node.
3. Pod spec with proper toleration block and key value with effects configured like NoSchdule, PreferNoSchedule, NoExute are added.
4. Only pod with this specific toleration on them can be scheduled with by passing effects.
  - kubectl taint node node01 type=hp:NoSchdule 
   - kubectl taint node node01 type- to remove taint
# Daemonsets
1. making sure exactly one pod of application or if cluster has multple applications then every nodes has copy of application on every node across cluster for H/A.
2. As mentioned , if multple applications exits, only one copy of it made sure by daemonset tobe schedulde on every node.
3. If only application is running, then a copy of it can be found on evry node, given it also has been configured with **taint**,**tolerations**. 
4. Do node add Nodeaffinity, it defeats the purpose.
# Priorityclass
1. Defined with preemption value, any pod with less value will eb prempted(removed) to facilitate current pod with priority class value more than it.
2. preemptionPolicy are preemptlower, never, with specific pods referencing this class or can be set as global default across cluster resource.