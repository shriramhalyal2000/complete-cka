# roadmap on how to generate *.key, *.csr, *.crt and adding user to cluster
1. Genrate openssl key and csr
   - Generate an RSA 2048-bit private key and create a Certificate Signing Request (CSR).

    CN (Common Name): Sets the Kubernetes username (devuser).

    O (Organization): Sets the Kubernetes group (developers) used for RBAC bindings.

   1. Generate private key:
      $ openssl genrsa -out devuser.key 2048

   2. Generate CSR:
      $ openssl req -new -key devuser.key -out devuser.csr -subj "/CN=devuser/O=developers" 
2. Submit CSR to cluster
   - user-csr.yaml

    1. Encode CSR to a single-line base64 string
      $ export CSR_BASE64=$(cat devuser.csr | base64 | tr -d '\n')

    2. Create the Kubernetes CSR manifest
      $ cat < devuser-csr.yaml
        apiVersion: certificates.k8s.io/v1
        kind: CertificateSigningRequest
        metadata:
          name: devuser-csr
        spec:
          request: ${CSR_BASE64}
          signerName: kubernetes.io/kube-apiserver-client
          usages:
          - client auth
        EOF

    3. Apply manifest to the cluster
      $ kubectl apply -f devuser-csr.yaml
3. Approve CSR and extract *.crt
   1. View pending CSRs
      $ kubectl get csr

   2. Approve the request
      $ kubectl certificate approve devuser-csr

   3. Extract signed certificate
      $ kubectl get csr devuser-csr -o jsonpath='{.status.certificate}' | base64 --decode > devuser.crt

4. Adding user to cluster

  1. Configure Kubeconfig & Switch Context:Final configuration.Add the user's credentials (devuser.crt and devuser.key) to kubeconfig 
    and set up a new context to use them.Bash# 1. Get     existing cluster name
    CLUSTER_NAME=$(kubectl config view -o jsonpath='{.clusters[0].name}')

  2. Set credentials in kubeconfig (embedding certs keeps config self-contained)
    $ kubectl config set-credentials devuser \
    --client-certificate=devuser.crt \
    --client-key=devuser.key \
    --embed-certs=true

  3. Create a dedicated context for the new user
    $ kubectl config set-context devuser-context \
    --cluster=${CLUSTER_NAME} \
    --user=devuser

  4. Switch to the new user context
    $ kubectl config use-context devuser-context