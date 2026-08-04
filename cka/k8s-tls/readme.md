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
2. Signing for Admin user
 - $ openssl genrsa -out admin.key 2048 -> generate key
 - $ openssl req  -new - key admin.key -subj "/CN=Kube-admin" -out admin.csr -> csr to generate key sign by CA to requester.
 - $ openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt -> signed key put to  certificate by CA, then the relevent crt added to user group on cluster.
   - /CA=kube-admin/OU=System:Masters for adding certificate to k8s user group.
3. Signing User/Client certificates with appropriate names and assigned groups.
4. Same as Serve side Certificates are issues, so does the Api-server server/cleint certificates are issued seperately.