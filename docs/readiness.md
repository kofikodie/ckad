# Kubernetes Readiness Probe

The Kubernetes readiness probe is a mechanism that allows you to determine whether a container is ready to accept traffic or not. It is used by the Kubernetes control plane to determine the health of a container and decide whether it should receive traffic from the service.

## Why Use Readiness Probes?

Readiness probes are essential for ensuring that only healthy containers receive traffic. By using readiness probes, you can prevent traffic from being routed to containers that are not yet ready to handle requests. This helps to maintain the overall stability and availability of your application.

## Configuring Readiness Probes

To configure a readiness probe for a container, you need to define it in the pod specification. Here's an example of how to configure a readiness probe using an HTTP GET request:

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
        readinessProbe:
        httpGet:
            path: /api/ready
            port: 80
    ```
````

## Types of Readiness Probes

Kubernetes supports the following types of readiness probes:

- **httpGet**: Sends an HTTP GET request to the specified path and port. If the response status code is in the range 200-399, the container is considered ready.

- **tcpSocket**: Opens a TCP connection to the specified port. If the connection is successful, the container is considered ready.

- **exec**: Executes a command inside the container. If the command exits with a status code of 0, the container is considered ready.

### Additional Options

You can configure additional options for readiness probes, such as the initial delay before the probe is initiated, the interval between probes, the timeout for each probe, and the number of retries before the container is marked as unhealthy.

Here's an example of a readiness probe with additional options:

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
        readinessProbe:
        httpGet:
            path: /api/ready
            port: 80
        initialDelaySeconds: 5
        periodSeconds: 10
        timeoutSeconds: 5
        successThreshold: 1
        failureThreshold: 3
    ```
````

In this example, the readiness probe will start 5 seconds after the container starts, run every 10 seconds, have a timeout of 5 seconds, require 1 successful probe to mark the container as ready, and allow up to 3 failed probes before marking the container as unhealthy.
