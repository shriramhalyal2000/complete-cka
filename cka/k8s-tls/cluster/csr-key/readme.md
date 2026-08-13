# create a new user an dvalid date certificate


Step 1: Generate a Private Key and CSR LocallyThe new user needs an SSL private key and a Certificate Signing Request (CSR). Run these commands on your terminal using OpenSSL:bash# 1. Generate a 2048-bit private key
openssl genrsa -out developer.key 2048

# 2. Create the CSR. The "CN" (Common Name) will be the Kubernetes username.
openssl req -new -key developer.key -out developer.csr -subj "/CN=developer/O=dev-team"
Use code with caution.(Note: O stands for Organization, which Kubernetes treats as a Group name if you want to apply group-level permissions later).
Step 2: Submit the CSR to the Kubernetes ClusterTo get the cluster's internal CA to sign the certificate, you must submit a Kubernetes CertificateSigningRequest object.Convert your local .csr file into a single line of Base64 text:bashcat developer.csr | base64 | tr -d '\n'
Use code with caution.Create a file named csr.yaml and paste that base64 string into the request field:yamlapiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: developer-user-csr
spec:
  request: <PASTE_BASE64_STRING_HERE>
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # 1 day
  usages:
  - client auth
Use code with caution.Apply the resource to the cluster:bashkubectl apply -f csr.yaml
Use code with caution.Step 3: Approve the Request and Retrieve the CertificateAn administrator must explicitly approve the certificate request before it can be used.bash# 1. Approve the request
kubectl certificate approve developer-user-csr

# 2. Extract the issued certificate from the cluster and save it locally
kubectl get csr developer-user-csr -o jsonpath='{.status.certificate}' | base64 --decode > developer.crt
Use code with caution.Step 4: Configure the User's Kubeconfig FileNow that you have developer.key and developer.crt, configure a specific access profile (context) so kubectl can interact with the cluster as this user.bash# 1. Add the user credentials to kubeconfig
kubectl config set-credentials developer \
  --client-certificate=developer.crt \
  --client-key=developer.key \
  --embed-certs=true

# 2. Create a specific context linking the cluster to this new user
kubectl config set-context developer-context \
  --cluster=<YOUR_CLUSTER_NAME> \
  --user=developer \
  --namespace=default
Use code with caution.Step 5: Grant Permissions via RBACBy default, the new user can authenticate but has zero access to view or modify resources. 
You must bind a Role or ClusterRole to them.To grant the user specific permissions (such as pod management in the default namespace), create and apply an RBAC Role and RoleBinding configuration (rbac.yaml) using kubectl apply -f rbac.yaml.
Step 6: Test the AccessSwitch to the new user context to verify your configuration:Run kubectl config use-context developer-context to switch contexts.Test allowed access with kubectl get pods and confirm restricted access fails as expected with kubectl get deployments.
Switch back to your admin context using kubectl config use-context <admin-context-name>.Let me know if you need help with cluster-wide access, external identity providers (OIDC/AWS IAM), or ServiceAccounts.