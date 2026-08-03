# to demostrate schedulinjg class of kubernetes scheduling pod with different examples
1. Mannuallu Scheduling
2. Node selector
3. nodeAffinity
4. Taint and tolerations
5. Daemonsets
6. Static pods
7. Priority class
# label node
1. kubectl label node <node-name> <key>=<value> to label it with desired key and value, 
2. kubectl label node <node-name> <key>- to unlabel it.
 - this labels can be put into pods by pod sec: nodeName/nodeSelector properties