# Deployment.yaml

    apiVersion: apps/v1        # API version used
    kind: Deployment           # Type of resource
    metadata:                  # Metadata about the deployment
       name: my-app-deployment  # Name of the deployment
       labels:
          app: my-app            # Label to identify the deployment
    spec:                      # Specification of the deployment
       replicas: 3              # ReplicaSet
       selector:                # How to identify pods managed by this deployment
         matchLabels:
           app: my-app
       template:                # Pod template
         metadata:
           labels:
              app: my-app        # Label for pods
         spec:                  # Specification for the pod
           containers:
              - name: my-app-container
                image: nginx:latest         # Container image to run
                ports:
                 - containerPort: 80       # Port exposed by the container


### Meaning Of Each Line Of Instruction: 
| Field        | Description                                    |
| ------------ | ---------------------------------------------- |
| `apiVersion` | API version (`apps/v1` for deployments)        |
| `kind`       | Resource type (`Deployment`)                   |
| `metadata`   | Name and labels for identification             |
| `spec`       | Defines the desired state (replicas, selector) |
| `template`   | Template for pods this deployment will create  |
| `containers` | List of containers inside each pod             |
| `image`      | Docker image used in the container             |
| `ports`      | Exposed container port                         |


# Commands
| #  | Command                                                      | Description                                                               |
| -- | ------------------------------------------------------------ | ------------------------------------------------------------------------- |
| 1  | `kubectl get pods`                                           | Lists all pods in the current namespace.                                  |
| 2  | `kubectl get deployments`                                    | Lists all deployments.                                                    |
| 3  | `kubectl get services`                                       | Lists all services.                                                       |
| 4  | `kubectl apply -f <file>.yaml`                               | Creates or updates resources defined in the YAML file.                    |
| 5  | `kubectl delete -f <file>.yaml`                              | Deletes resources defined in the YAML file.                               |
| 6  | `kubectl describe pod <pod-name>`                            | Shows detailed information about a specific pod.                          |
| 7  | `kubectl logs <pod-name>`                                    | Displays logs for a pod's main container.                                 |
| 8  | `kubectl exec -it <pod-name> -- bash`                        | Executes an interactive shell inside the pod.                             |
| 9  | `kubectl scale deployment <name> --replicas=N`               | Scales a deployment to N replicas.                                        |
| 10 | `kubectl expose deployment <name> --type=NodePort --port=80` | Exposes a deployment as a service on a node port.                         |
| 11 | `kubectl get all`                                            | Lists all resources (pods, services, deployments, etc.) in the namespace. |
| 12 | `kubectl edit deployment <name>`                             | Opens the deployment configuration in a text editor for live changes.     |
| 13 | `kubectl port-forward pod/<pod-name> 8080:80`                | Forwards port 80 from the pod to localhost:8080.                          |
| 14 | `kubectl get namespaces`                                     | Lists all namespaces in the cluster.                                      |
| 15 | `kubectl config set-context --current --namespace=<ns>`      | Sets the default namespace for your kubectl context.                      |
