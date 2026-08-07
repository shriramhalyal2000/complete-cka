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
2. Signing for Admin user references ca 
 - $ openssl genrsa -out admin.key 2048 -> generate key
 - $ openssl req  -new - key admin.key -subj "/CN=Kube-admin" -out admin.csr -> csr to generate key sign by CA to requester.
 - $ openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt -> signed key put to  certificate by CA, then the relevent crt added to user group on cluster.
   - /CA=kube-admin/OU=System:Masters for adding certificate to k8s user group.
3. Signing User/Client certificates with appropriate names and assigned groups.
4. Same as Serve side Certificates are issues, so does the Api-server server/cleint certificates are issued seperately.
5. Above lists the mannual way to assign, sign, and verify TLS certificates, But using KUBEADM its dome automatically configured in kubeconfig.
6. Certificates path $ cat /etc/kubenetes/manifest/kube-apiserver.yaml 
   - --tls-cert-file=/etc/kubenetes/manifests/kube-spaiserver.crt -> in kubeconfig
7. For a certificate to be issued, for a server, all its DNS(Domain Name Server have to be issued,
     with Its Alias AAAA records so serves with these records are also redirect links considerd original server).
8. But in this kubenetes case, kube-apiserver has many alias names as kube-apiserver
# create and approve signing request for new cluster usiong using certifcates.k8s.io/v1
1. Create Kind: CertificateSigningRequest
2. Embed **user**.csr file with base64 value in request
3. Assign user to systemGroup
4. Commands
   - kubectl get certificates -o yaml
   - kubectl get csr //get csr approved pending or denied
   - kubectl certificate approve requester
   - kubectl certificate deny requester
   - kubectl delete csr name-csr to delete csr from cluster