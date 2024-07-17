# Authentication, Authorization, and Admission Control in Kubernetes

Authentication, authorization, and admission control are crucial aspects of securing a Kubernetes cluster. Let's explore each of these concepts:

## Authentication

Authentication in Kubernetes verifies the identity of users, processes, and components attempting to access the cluster. Kubernetes supports various authentication mechanisms, including:

- Client certificates: Users present a valid client certificate to authenticate themselves.
- Static token files: Users provide a token that is compared against a list of valid tokens.
- OpenID Connect (OIDC): Users authenticate using an external identity provider that supports OIDC.
- Service accounts: Pods and other cluster components authenticate using service account tokens.

## Authorization

Authorization in Kubernetes determines what actions users, processes, and components are allowed to perform within the cluster. Kubernetes uses Role-Based Access Control (RBAC) to define authorization policies. RBAC allows cluster administrators to grant permissions to users and groups based on roles and role bindings.

Roles define a set of permissions, such as creating or deleting resources, while role bindings associate roles with users or groups. By configuring RBAC, administrators can control access to various resources and operations within the cluster.

### Authorization Mechanisms

Kubernetes provides several authorization mechanisms, including:

- Node authorizer: Authorizes kubelets to access the API server.
- ABAC (Attribute-Based Access Control): Allows administrators to define policies based on attributes.
- RBAC: Provides a declarative way to manage authorization policies based on roles and role bindings.
- Webhook: Allows external systems to make authorization decisions.

### Authorization Modes

Kubernetes supports multiple authorization modes, such as:

- AlwaysDeny: Denies all requests.
- AlwaysAllow: Allows all requests.
- ABAC: Attribute-Based Access Control.
- Webhook: External systems make authorization decisions.
- RBAC: Role-Based Access Control.
- Node: Authorizes kubelets to access the API server.

When multiple authorization modes are enabled, Kubernetes uses the most permissive mode to authorize requests.

## Admission Control

Admission control in Kubernetes enforces policies and rules before allowing objects to be created or modified in the cluster. It acts as a gatekeeper, validating requests and ensuring they comply with predefined rules. Admission controllers can be used to enforce security policies, resource limits, and custom business logic.

Kubernetes provides several built-in admission controllers, such as `NamespaceLifecycle`, `ResourceQuota`, and `PodSecurityPolicy`. Additionally, custom admission controllers can be developed to enforce specific policies based on the cluster's requirements.

By combining authentication, authorization, and admission control, Kubernetes provides a robust security framework for managing access and ensuring the integrity of the cluster.

Follow the below instructions to configure basic authentication in a kubeadm setup.

Create a file with user details locally at `/tmp/users/user-details.csv`

```txt
password123,user1,u0001
password123,user2,u0002
password123,user3,u0003
password123,user4,u0004
password123,user5,u0005
```

Edit the kube-apiserver static pod configured by kubeadm to pass in the user details. The file is located at `/etc/kubernetes/manifests/kube-apiserver.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
    - command:
        - kube-apiserver
          <content-hidden>
      image: k8s.gcr.io/kube-apiserver-amd64:v1.11.3
      name: kube-apiserver
      volumeMounts:
        - mountPath: /tmp/users
          name: usr-details
          readOnly: true
  volumes:
    - hostPath:
        path: /tmp/users
        type: DirectoryOrCreate
      name: usr-details
```

Modify the kube-apiserver startup options to include the basic-auth file

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
    - command:
        - kube-apiserver
        - --authorization-mode=Node,RBAC
          <content-hidden>
        - --basic-auth-file=/tmp/users/user-details.csv
```

Create the necessary roles and role bindings for these users:

````yaml
        ---
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: pod-reader
        rules:
        - apiGroups: [""] # "" indicates the core API group
          resources: ["pods"]
          verbs: ["get", "watch", "list"]

        ---
        # This role binding allows "jane" to read pods in the "default" namespace.
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: read-pods
          namespace: default
        subjects:
        - kind: User
          name: user1 # Name is case sensitive
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role #this must be Role or ClusterRole
          name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
          apiGroup: rbac.authorization.k8s.io
          ```
````

Once created, you may authenticate into the kube-api server using the users credentials

`curl -v -k https://localhost:6443/api/v1/pods -u "user1:password123"`

### Configuring Admission Webhooks

Admission webhooks in Kubernetes allow you to intercept and validate requests to the API server before they are persisted. This enables you to enforce custom policies, validate configurations, and perform additional checks on resources before they are created or modified.

Admission webhooks consist of two types: validating admission webhooks and mutating admission webhooks.

- Validating admission webhooks validate and potentially reject requests based on predefined rules or policies. They can be used to enforce security policies, resource limits, naming conventions, and other constraints.

- Mutating admission webhooks modify requests before they are persisted to the cluster. They can be used to automatically inject sidecar containers, set default values, or perform other transformations on resources.

To configure admission webhooks in Kubernetes, you need to create a webhook server that implements the necessary logic for validating or mutating requests. The webhook server should expose an HTTPS endpoint that the API server can communicate with.

Once the webhook server is running, you can register it with the API server by creating a `ValidatingWebhookConfiguration` or `MutatingWebhookConfiguration` object. These objects specify the webhook server's endpoint, the resources it should intercept, and the rules it should enforce.

Example of a ValidatingWebhookConfiguration object:

```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: example-validating-webhook
webhooks:
  - name: example.validating.webhook.com
    clientConfig:
      service:
        name: example-validating-webhook
        namespace: default
    rules:
      - operations: ["CREATE", "UPDATE"]
        apiGroups: [""]
        apiVersions: ["v1"]
        resources: ["pods"]
    admissionReviewVersions: ["v1"]
```

In this example, we define a `ValidatingWebhookConfiguration` object that specifies a validating webhook for pods. The webhook server is expected to be running as a service named `example-validating-webhook` in the `default` namespace. The webhook will intercept `CREATE` and `UPDATE` operations on pods in the `v1` API version.

## Managing Roles and Role Bindings

Roles and RoleBindings are essential components of Kubernetes RBAC (Role-Based Access Control) that allow you to define and manage permissions within a cluster. Roles define a set of permissions that can be applied to resources, while RoleBindings associate roles with users, groups, or service accounts.

### Creating a Role

To create a Role in Kubernetes, you need to define the permissions that the Role will grant. Here's an example of a Role manifest that allows read access to Pods in the `default` namespace:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
```

In this example manifest we define a Role named `pod-reader` that grants read access to Pods in the `default` namespace. The `rules` section specifies the permissions, including the API groups, resources, and verbs that are allowed.

### Creating a RoleBinding

Once you have defined a Role, you can create a RoleBinding to associate the Role with users, groups, or service accounts. Here's an example of a RoleBinding manifest that binds the `pod-reader` Role to a user named `jane`:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
  - kind: User
    name: jane
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

In this example manifest, we create a RoleBinding named `read-pods` that binds the `pod-reader` Role to the user `jane` in the `default` namespace. The `subjects` section specifies the user to which the Role is bound, and the `roleRef` section references the Role that is being bound.

### ClusterRole and ClusterRoleBinding

In addition to Roles and RoleBindings, Kubernetes also supports ClusterRoles and ClusterRoleBindings, which apply permissions across the entire cluster rather than a specific namespace. ClusterRoles and ClusterRoleBindings are useful for defining global permissions that apply to all namespaces.

To create a ClusterRole or ClusterRoleBinding, you can use the same manifests as shown above, but without specifying a `namespace` field. This indicates that the permissions apply cluster-wide rather than to a specific namespace.
