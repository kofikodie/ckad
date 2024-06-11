# Metric Server

The Metric Server is a component in Kubernetes that collects resource usage metrics from the cluster's nodes and pods. It provides these metrics to other components, such as the Horizontal Pod Autoscaler (HPA), which uses the metrics to scale the number of replicas for a given workload.

## Installation

To install the Metric Server, follow these steps:

1. Download the Metric Server YAML file from the official Kubernetes GitHub repository.

2. Apply the YAML file using the `kubectl apply` command:

   ```shell
   kubectl apply -f metric-server.yaml
   ```

3. Verify that the Metric Server is running by checking the pods:

   ```shell
   kubectl get pods -n kube-system
   ```

## Usage

Once the Metric Server is installed, you can use it to retrieve resource usage metrics for your pods and nodes. Here are a few examples:

- To get the CPU usage of all pods in the cluster:

  ```shell
  kubectl top pods --all-namespaces
  ```

- To get the memory usage of a specific pod:

  ```shell
  kubectl top pod <pod-name> -n <namespace>
  ```

- To get the CPU usage of all nodes in the cluster:

  ```shell
  kubectl top nodes
  ```

## Troubleshooting

If you encounter any issues with the Metric Server, here are a few troubleshooting steps you can try:

1. Check the logs of the Metric Server pod:

   ```shell
   kubectl logs -n kube-system <metric-server-pod-name>
   ```

2. Ensure that the Metric Server is properly configured and has access to the necessary resources.

3. Verify that the kubelet on each node is properly configured to expose metrics to the Metric Server.

For more information and advanced usage of the Metric Server, refer to the official Kubernetes documentation.

## Conclusion

The Metric Server is a crucial component in Kubernetes for collecting and providing resource usage metrics. By installing and using the Metric Server, you can enable autoscaling and make informed decisions about resource allocation in your cluster.
