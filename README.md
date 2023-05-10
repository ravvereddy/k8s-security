# k8s-security
- When writing Kubernetes manifests, there are several security best practices to consider to enhance the security of your Kubernetes deployments. Here are some key practices along with examples:

# Use Role-Based Access Control (RBAC):

RBAC restricts access to Kubernetes resources based on user roles and permissions. Limiting access helps prevent unauthorized actions.
Example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: my-role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: my-role-binding
subjects:
- kind: User
  name: user1
roleRef:
  kind: Role
  name: my-role
  apiGroup: rbac.authorization.k8s.io
```

# Limit Pod Capabilities:

- Restrict the capabilities of containers within pods to prevent potential privilege escalation.
Example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: my-image
    securityContext:
      capabilities:
        drop: ["ALL"]
      # Additional container configuration...
```

# Enable Pod Security Policies (PSP):

- PSPs define a set of security policies that pods must adhere to, including restrictions on privileged operations and resource usage.
Example:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: my-psp
spec:
  privileged: false
  # Additional security policy configuration...
```

# Use Network Policies:

- Network policies allow you to define rules for inbound and outbound traffic to pods, ensuring that only authorized network connections are allowed.
Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy
spec:
  podSelector:
    matchLabels:
      app: my-app
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: backend
    ports:
    - protocol: TCP
      port: 80
```      
# Enable Pod Security Context:

- Utilize the Pod Security Context to set security-related configurations for pods, such as the user and group IDs, and file system permissions.
Example:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  securityContext:
    runAsUser: 1000
    fsGroup: 2000
    # Additional security context configuration...
  # Container specification...
```  

These examples demonstrate some of the security best practices when writing Kubernetes manifests. However, it's important to consider the specific security requirements of your applications and infrastructure, and adjust the configurations accordingly. Regularly reviewing and updating your manifests based on evolving security best practices is crucial for maintaining a secure Kubernetes environment.
