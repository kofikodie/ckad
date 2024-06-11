# Node Affinity in Kubernetes

Node affinity is a feature in Kubernetes that allows you to specify rules for scheduling pods onto specific nodes. It enables you to control the placement of pods based on various criteria, such as node labels, node taints, and pod affinity/anti-affinity.

## Node Selector

One way to use node affinity is through node selectors. Node selectors allow you to specify a set of key-value pairs that must match the labels on a node for a pod to be scheduled on that node.

To use node selectors, you need to:

1. Add labels to your nodes using the `kubectl label` command.
2. Specify the node selector in the pod's YAML file using the `nodeSelector` field.

Here's an example of a pod YAML file that uses node selectors:

```shell
kubectl label nodes node1 disktype=ssd
```

````yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
    containers:
    - name: mycontainer
        image: nginx
    nodeSelector:
        disktype: ssd
    ```
````

In this example, the pod `mypod` has a node selector that specifies the key `disktype` with the value `ssd`. This means that the pod will only be scheduled on nodes that have the label `disktype=ssd`.

## Node Affinity

Node affinity provides a more flexible way to control pod placement by allowing you to specify rules based on node labels. You can use node affinity to specify either required or preferred rules for pod scheduling.

To use node affinity, you need to:

1. Specify the node affinity rules in the pod's YAML file using the `affinity` field.
2. Define the node affinity rules using the `requiredDuringSchedulingIgnoredDuringExecution` or `preferredDuringSchedulingIgnoredDuringExecution` fields.

Here's an example of a pod YAML file that uses node affinity:

```shell
kubectl label nodes node1 disktype=ssd
```

````yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
    containers:
    - name: mycontainer
        image: nginx
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                    - key: disktype
                        operator: In
                        values:
                        - ssd
    ```
````

In this example, the pod `mypod` has a node affinity rule that specifies that it should be scheduled on nodes with the label `disktype=ssd`.

Node affinity is a powerful feature that allows you to fine-tune the placement of pods in your Kubernetes cluster based on specific criteria. By using node selectors and node affinity, you can control where your pods are scheduled and optimize resource allocation in your cluster.
[]: # (END)
[]: # Node Affinity in Kubernetes
