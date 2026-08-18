# configuring autosaclingpod resource for any deployment/replicaset.
1. Since manually scalking deploymenty or replicaset with scale command is difficult as to monitor manually pod resource consumption and load.
2. Configuring Horizontal Pod Autoscaler with mertics server, triggers auto-up-scaling and auto-dowb-scaling with metrics type.
3. Defining a HPA resource kind, and adding Deployment/Replicaset tot it to monitor and trigger automatically.