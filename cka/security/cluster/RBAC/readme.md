# cluster with multi user platform
1. RBAC- Role based Access Control.
2. ABAC - Atribute based access control.
3. Node - node based access control
4. Webhook
Defining least previllages with max outcomes.

1. In RBAC, user is defined in kubeconfig, placed in $HOME/.kube/manifest dir, 
  - housed with user, user certificate, key, and attributes that can be used by user toaccess cluster, 
  - get, describe, -w create, depends on cluster business logic