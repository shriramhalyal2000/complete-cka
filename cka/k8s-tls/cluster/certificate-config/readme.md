# CSR
1. Certificate signing Request is raised by user with openssl key gen put in to file with base64 encoded
2. The user with adminperms approves user request to grant access to user.
3. And certificate is genrated
   - 1. Approve the pending CSR
        kubectl certificate approve <csr-name>

   - 2. Extract and decode the generated user certificate
        kubectl get csr <csr-name> -o jsonpath='{.status.certificate}' | base64 --decode > user.crt
   - 3. openssl x509 -req \
        -in user.csr \
        -CA ca.crt \
        -CAkey ca.key \
        -CAcreateserial \
        -out user.crt \
        -days 365 \
        -sha256
