# Liveness Probe

The liveness probe is a Kubernetes feature that allows you to check the health of your application running inside a container. It helps Kubernetes determine if the container is running as expected and whether it needs to be restarted.

## Why Use Liveness Probes?

Liveness probes are essential for ensuring that your application is running correctly and is able to handle requests. By using liveness probes, you can detect when your application is in a bad state and automatically restart it to restore its health. This helps to maintain the overall stability and availability of your application.

## Configuring Liveness Probes

To configure a liveness probe, you need to define it in the pod specification. Here's an example of how to configure a liveness probe using an HTTP GET request:

````yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
    containers:
    - name: my-container
        image: nginx:latest
        ports:
        - containerPort: 80
        livenessProbe:
        httpGet:
            path: /api/health
            port: 80
    ```
````

## Types of Liveness Probes

Kubernetes supports the following types of liveness probes:

- **httpGet**: Sends an HTTP GET request to the specified path and port. If the response status code is in the range 200-399, the container is considered healthy.

- **tcpSocket**: Opens a TCP connection to the specified port. If the connection is successful, the container is considered healthy.

- **exec**: Executes a command inside the container. If the command exits with a status code of 0, the container is considered healthy.

### Additional Options

You can configure additional options for liveness probes, such as the initial delay before the probe is initiated, the interval between probes, the timeout for each probe, and the number of retries before the container is marked as unhealthy.

Here's an example of a liveness probe with additional options:

````yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
    containers:
    - name: my-container
        image: nginx:latest
        ports:
        - containerPort: 80
        livenessProbe:
        httpGet:
            path: /api/health
            port: 80
        initialDelaySeconds: 5
        periodSeconds: 10
        timeoutSeconds: 5
        failureThreshold: 3
    ```
````

In this example, the liveness probe sends an HTTP GET request to the `/api/health` path on port 80. It has an initial delay of 5 seconds before the probe is initiated, a probe interval of 10 seconds, a probe timeout of 5 seconds, and a failure threshold of 3 retries before the container is marked as unhealthy.
