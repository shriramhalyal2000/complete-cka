# This is a kind cluster setup to perform cka TLS POC

* certificate (public)
  - clint.crt
  - client.pem
  - server.crt
  - server.pem

* private key
 - client.key
 - client-key.pem
 - server.key
 - server-key.pem // for *.pem files **key** is mentioned, and any file with **key** inits name is automatically a private key.

1. Commands to create certofocates by user.
 - $ openssl genrsa -out adm.key 2048
 - $ openssl req -new -key adm.key  -out adm.csr -subj "/CN=adm"
 - $ cat *csr | base64 -w 0 >> csr.txt to get encode value in single line, and put this value in spec.request: in user csr requester.yaml
 - $ kubectl apply -f *.yaml for generating user request
 - $ kubectl get csr , find pending csr
 - $ kubectl certificate apprive/deny csr-name-pending
 - $ kubectl delete certificate csr-name to revoke status
2. This POC shows a admin perm user manually approves the csr, raised by user to kube-apiserver-client, but in prod grade a CA server approves csr raised by new user.
  - since default-user is Kubenetes-admin inkind cluster, cluster user has to mannually approve it. 
# kubeconfig is generated when cluster is created, 
1. of using resource config cluster is generated , if kubeadmini used, seperate resource.yaml file created with certificate loaded and their location.
2. In kind cluster you only need cluster certificate. 
3. Kubeconfig can be found in >>$HOME/.kube dir of vm/server
4. It can also be configured what can user preform in cluster, like get pods, get node, "get", "apply", "replace", "describe",
   these verb can be implimented to user so only desired actions  can be performed with least previllages