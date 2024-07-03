# API Group

In Kubernetes, an API Group is a collection of related resources. It provides a way to organize and manage different types of resources within the Kubernetes cluster.

API Groups are represented by a unique name and are accessed using a specific API endpoint. Each API Group has its own set of resources, which can include objects like Pods, Services, Deployments, and more.

By using API Groups, Kubernetes allows for extensibility and modularity, as different teams or projects can define their own custom resources within their respective API Groups.

To interact with resources within an API Group, you can use the `kubectl` command-line tool or make API requests directly.

## Cluster Functionality

Cluster functionality is managed by two categories of APIs: the `core` group (also known as /api) and the `named` group (also known as /apis)

The `core` group contains the core Kubernetes resources, such as Pods, Services, Namespaces, and Nodes. These resources are available by default in every Kubernetes cluster.

The `named` group contains custom resources that are defined by users or third-party developers such as deployments, statefulsets, storage classes, authorization policies, and more. These resources are organized into different API Groups based on their functionality.
