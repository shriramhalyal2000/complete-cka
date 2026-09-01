#  configuring pod netowrking policy.
- by default k8s is configured to have every pod on node to have access to every othe rpod on node.
- but this raises security concers as to have unwanted access to have on pods that can be easily manipulated.
- ingress and egress from and to a pod can bve configrued.
- the target pod has ingress, which defines who can acess that pod on what pod.
- and the mark pod is the one trying to access the pod on target pod port. and can have egress defined on it.
- but in general ingress and egress security policy on pod ports can also be defined.
  - as one application is , the front-end pod can never acess db-pod on port 3306, and the ingress and egress on pod db-pod is blocked for ingress of front-end pod.
  - only backend-pod can have egress on db-pod ingress route on port 3306, and db pod accepts ingress on port 3306 from backend pod,
   and db sends back egress traffic from db-pod to backednpod as ingress for it on its designated port.
- and these netowrk policy are namespaced.
- attached netowrk policy demonstrate the following:
  - network policy is applied on msql-pod.
  - intention is to havemysql pod accept traffic on its own port 3306 by pod-backend.
  - and send back traffic to pod-backend on port of backend 8000 from mysql-pod.
- this netowrk policy also accepts cidr range ip block along with podSelector argument.