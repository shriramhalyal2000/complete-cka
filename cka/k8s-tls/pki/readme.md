# this is a poc to impliment TLS in KIND(Kubenetes in Docker) setup
1. Implement, user(client), Api-Server(Server) encryption.
2. Api-Server(Client)-Kubelet(node-server) encryption.
3. Add new users to vcluster, rais CSr,Approve/Delete/Deny 
4. Every client has a public certificate generated from $ openssl gen command