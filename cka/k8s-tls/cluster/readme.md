# this is a kind cluster setup to perform cka TLS POC

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
