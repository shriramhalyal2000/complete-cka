# TLS for k8s components
1. K8s uses HTTP/S REST API for communication between api server and other components like kubelet, scheduler, controller, and grpc for etcd.
2. So securing communication in flight is also necessary for kubenetes security.
3. In Prod there is private CA server established with privately signed certificates issued to resource for TLS, 
    and resource acting as client with authenticate with requesting CA server  about certificate authencity before establishing secure connection. 
# Signing certificates and creating keys and signing certificates for k8s cluster
1. for CA for cluster to authenticate users and servers.
   - $ openssl genrsa -out **name**.key 2048
   - $ openssl req -new -key **name**.key -subj "/CN=KUBERNETES-CA" -out **name**.csr
   - $ openssl x509 -req -in **name**.csr -signkey **name**.key -out **name**.crt
   - you can add **name**=ca for Certificate Authority.