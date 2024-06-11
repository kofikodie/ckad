# Service Account

A service account is an identity used by pods to authenticate and authorize their actions within a Kubernetes cluster.

## Creating a Service Account

To create a service account, you can use the `kubectl` command-line tool or define a YAML manifest.

### Using `kubectl`

To create a service account using `kubectl`, run the following command:

```shell
kubectl create serviceaccount <service-account-name>
```

### Using a YAML Manifest

You can also define a service account using a YAML manifest. Here is an example of a service account definition:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: <service-account-name>
```

To create a secret using the YAML manifest, run the following command:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: <secret-name>
type: kubernetes.io/service-account-token
```

Link the service account to the secret by adding the following field to the service account definition:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: <service-account-name>
secrets:
  - name: <secret-name>
```

## Using a Service Account

To use a service account in a pod, you need to specify the service account name in the pod's spec section. Here is an example of a pod definition that uses a service account:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: <pod-name>
spec:
    serviceAccountName: <service-account-name>
    containers:
        - name: <container-name>
        image: <container-image>
    ```
```
