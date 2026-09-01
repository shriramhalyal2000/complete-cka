# priority class
1. For pods with low value priority will be pre-empted wby pods with high priority value.
   - Value ranges from -2.147 billion to + 1 billion.
   - Pods with value priority class say, 1 million wiill be pre-empted by 10 million.
   - In scheduler, when pods scheduling registred, this value is checked, during execution.
   - when a priority class is applied, shceduler checks wheather its global default, 
     or pods with specific priorityClassName metioned in pod spec.

2. Pod readiness, liveness, and startup probe.
   - Startup Probe: 
     - This checks wheather th e underlysing container has started or not.
     - health checks are configured with  touch /tmp/healthz 
     - exec command, to check container health in designated dir path of container.
     - that health checks can be delayed, as pod container is niot actualyy running the second it scheduled, 
       pod container has its own processing, of waiting fopr other services to be running, db connection, cach warm, and underlying network connection
       including, dependency installation.
     - this initialDelaySeconds, defines that how much should each probe wait before executing health checks. periodSeconds, and TCP ports can also be scheduled.
   - Liveness Probe:
     - when a app container is in deadlock, uses to restart container to get container off out deadlock state.
     - container is not ready but executed and running, its not yet accepting user requests,
   - Readiness probe:
     - When container has been created, its dependency installtion executed, 
       but its not yet started because its other backend services are not been connected, container is live but not ready.
   - health check methods
    1. httpGet method 
    livenessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5 # how much time it shoyuld wait before executing httpGet 
      periodSeconds: 10 # defines interval set for this methods on pod
      timeoutSeconds: 1 # time after probe times out
      failureThreadhold: 3 # homany failure can it sustain before not restarting and letting it crash
    2. Copmmand exec method:
      - curl command sent inside container by kubeclet
       livenessProbe:
        exec:
          command:
            - curl
            - -f
            - http://localhost:80/
        initialDelaySeconds: 5
        periodSeconds: 10
    3. Web socket method:
       - livenessProbe:
            tcpSocket:
            port: 80
        initialDelaySeconds: 5
        periodSeconds: 10