# use docker security for private registry image pull

- $ kubectl create secret docker-registry secret-name \
    --docker-username=docker_usr \
    --docker-password=dock_passwd \
    --docker-server=server.com:5000 \
    --docker-email=docker@email.com
- reference this sercret-name in pod container to pull image from private registry
- secret type is docker-resgistry built in kubernetes secret
- no need to use volume or any other

- to further ensure use non root user in DOCKERFILE USER argument for executing non root user.
- USER 1001 this makes any one if logged in doesnot have valid perms , cant access furhter anything
- this can be implimented at pod level and container level.
- securityCOntext:{} could be set at pod level and at container level.
- the same user can be added in pod or container.
  - spec.container:
           securityContext:
             capabilities:
               add: ["SYS_USER"]
               drop: ["all"]
           runAsNonRoot: true
           runAsUser: 1001
           runAsGroup: 200
  - spec:
    securityContext:
      runAsUSer: 1001 and other arguments can be configured from k8s doc ref.
      runAsNonRoot: true
      runAsGroup: 200
- pod level configurations override container level security context.