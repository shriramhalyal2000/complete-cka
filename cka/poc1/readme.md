# performing a poc to show kubernetes cluster with application deployemnt with necessary configuration and resource

1. Kind cluster with 3 nodes and ports mapped.
2. Install ingress controller on cluster and deine ingress resource.
3. condigure deployment and service for simple nginx app to serve html page
4. ping localhost:8080/ to accss cluster
5. Configure ingress resource path

- $ kubectl apply -f cluster.yaml
- $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml 
   - for nginx ingress controller with ingress-nginx namespace
- $ kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
  - wait for nginx ingress controller to be ready
- $ kubectl label node dev-worker ingress-ready=true --overwrite 
    to schedule ingress on node dev-worker.
- $ kubectl patch deployment ingress-nginx-controller -n ingress-nginx --type='json' -p='[
  {
    "op": "add",
    "path": "/spec/template/spec/nodeSelector",
    "value": {"kubernetes.io/hostname": "dev-worker"}
  }
  ]'
  - a live patch that sticks ingress controller pod on dev-worker node
  - dont forget to add 
    - $ kubectl create secret docker-registry reg-name \
        --docker-server=https://index.docker.io/v1/ \
        --docker-email=your-email-docker-@email.com \
        --docker-username=doc_usr \
        --docker-password=dock_passwd -> before deploying app.
- kubectl apply of deployment.yaml service.yaml ingress.yaml