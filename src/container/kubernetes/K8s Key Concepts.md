# Some Important key concepts in K8s

### ReplicaSet
- We, Developer, do not required to explicitly create the ReplicaSet.
- We work at the `Deployment.yml` abstraction level. **Deployments** automatically create ad manage `ReplicaSet` for us.
- Number of Pods in `.spec.replica` inside the `Deployment.yml` is the replicaset.
- `kind:ReplicaSet` 


### DaemonSet

### StatefulSet 

### Services

### Storage 

### Volumes

##### Storage Class

##### Volumes Types
- Persistent volumes
- projected Volumes
- Ephemeral Volumes

##### PVC(Persistent volumes Claim)
##### Dynamic Volume Provisioning


### Cronjob

### Ingress




