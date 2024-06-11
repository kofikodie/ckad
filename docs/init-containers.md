# FILEPATH

"""
This code snippet demonstrates the usage of init containers in Kubernetes.

Init containers are special containers that run and complete before the main containers in a pod start. They are used to perform initialization tasks, such as setting up configuration files or downloading data, before the main application starts.

In this example, we define an init container that runs a script to populate a shared volume with some initial data. The main container then uses this data during its execution.

Init containers are defined under the `spec` section of a pod's YAML configuration file. They are executed sequentially, and the main containers are started only after all init containers have completed successfully.

For more information on init containers in Kubernetes, refer to the official documentation: https://kubernetes.io/docs/concepts/workloads/pods/init-containers/
"""

# Define the YAML configuration for the pod

````yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-container-pod
spec:
    containers:
        - name: main-container
        image: nginx:latest
        volumeMounts:
            - name: shared-data
            mountPath: /usr/share/nginx/html
    initContainers:
        - name: init-container
        image: busybox:latest
        command: ['sh', '-c', 'echo "Hello, World!" > /work-dir/index.html']
        volumeMounts:
            - name: shared-data
            mountPath: /work-dir
    volumes:
        - name: shared-data
        emptyDir: {}
    ```
````
