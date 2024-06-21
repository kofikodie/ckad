# StatefulSet

A StatefulSet is a Kubernetes resource that manages the deployment and scaling of stateful applications. Unlike a Deployment, which is suitable for stateless applications, a StatefulSet provides guarantees about the ordering and uniqueness of Pods.

## Key Features

- **Stable Network Identifiers**: Each Pod in a StatefulSet has a stable network identifier (hostname) that persists across rescheduling and restarts. This allows applications to rely on stable network connections and enables stateful operations.

- **Ordered Deployment and Scaling**: StatefulSets ensure that Pods are deployed and scaled in a predictable order. This is important for applications that require specific sequencing or coordination during startup or shutdown.

- **Persistent Storage**: StatefulSets can be used with Persistent Volumes to provide persistent storage for stateful applications. Each Pod in a StatefulSet can have its own Persistent Volume, ensuring data persistence even if the Pod is rescheduled.

- **Headless Service**: StatefulSets automatically create a Headless Service, which provides DNS-based service discovery for the Pods. This allows other applications to discover and connect to individual Pods in the StatefulSet.

## Usage

To create a StatefulSet, you need to define a YAML manifest that specifies the desired state of the StatefulSet. Here's an example:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
    name: my-statefulset
spec:
    replicas: 3
    selector:
        matchLabels:
            app: my-app
    template:
        metadata:
            labels:
                app: my-app
        spec:
            containers:
            - name: my-container
                image: my-image
                ports:
                - containerPort: 8080
    serviceName: my-service
    volumeClaimTemplates:
    - metadata:
            name: data
        spec:
            accessModes: [ "ReadWriteOnce" ]
            resources:
                requests:
                    storage: 1Gi
```

In this example, we define a StatefulSet named "my-stateful set" with 3 replicas. The Pods in the StatefulSet are labeled with `app: my-app` and run a container named "my-container" with the image "my-image". The container exposes port 8080.

To create the StatefulSet, you can use the `kubectl apply` command:

```shell
kubectl apply -f statefulset.yaml
```

Once the StatefulSet is created, you can manage it using the standard Kubernetes commands, such as `kubectl scale` to scale the number of replicas or `kubectl delete` to delete the StatefulSet.
