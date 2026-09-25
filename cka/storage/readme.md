# Storage in kubernetes
- Volumes
- Persistant volumes
- persistant volume class
- ephimeral vol
- storage classes

- Persistent volumes:
  - created outside the cluster with CSI pluggin to connect external FS or storage to store pod data.
  - Kubernetes  has set of approved CSI pluggins, like aws EBS, Portworx etc.
  - These pluggins can be used as external FS to store pod data, rather than creating volume on cluster node.
  - The created volume can be claimed by pods, by defining these volume mounts with id and type.
  - The External volumes can be confgigured to extent of ReadWriteOnly, ReadWriteOnce.

- Persistent volume claims:
  - mounting persistent volumes created on to pods for data store, perform read write ops.
  - in EKS to add EBS as PV, need to create a ServiceAccount for EBS CSI driver, and add it to cluster, and INstall EBS CSI addon on cluster.
  - Create PV with EBS and PVC on pods.
  - Need to authenticate EBS with Cluster with OIDC(open id connect , iwth iam role and policy).
  - If the vol claim request is lesser than the available vol, k8s will will bind that vol with cliam, if nother option is available.
  - Once PVC request is initiatedno other pvc is allocate dthe volume untill its relased or deleted.
  - That Claim can be used to reference within pods, so pods can directly use PVC with PV bind for application.

- storage classes;
  - instead of manually provisioning PV, use storage class, outside cloud providers to automatically provision PV for claims.