# CKAD RBAC Practice Questions

## Application Environment, Configuration and Security (25%)

### Authentication and Authorization - RBAC (Role-Based Access Control)

---

## Question 1: Create Basic Role
Create a Role named `pod-reader` in namespace `dev` that allows:
- get, list, watch on pods
- get, list on services

---

## Question 2: Create RoleBinding
Create a RoleBinding named `read-pods` in namespace `dev` that:
- Binds the `pod-reader` Role
- To a ServiceAccount named `dev-user`

---

## Question 3: ClusterRole for All Namespaces
Create a ClusterRole named `pod-viewer` that allows:
- get, list, watch on pods in all namespaces

---

## Question 4: ClusterRoleBinding
Create a ClusterRoleBinding named `view-pods-global` that:
- Binds the `pod-viewer` ClusterRole
- To a user named `john@example.com`

---

## Question 5: Multiple Resources
Create a Role named `app-manager` in namespace `production` that allows:
- All verbs (get, list, watch, create, update, patch, delete) on deployments
- get, list, watch on pods
- get, list on configmaps and secrets

---

## Question 6: ServiceAccount with Role
Create:
- ServiceAccount named `app-sa` in namespace `default`
- Role named `deploy-manager` that can create, update, delete deployments
- RoleBinding to bind the Role to the ServiceAccount
- Pod that uses this ServiceAccount

---

## Question 7: Bind to Group
Create a RoleBinding named `dev-team-binding` that:
- Binds the Role `pod-reader`
- To a group named `dev-team`
- In namespace `development`

---

## Question 8: ClusterRole with Non-Resource URLs
Create a ClusterRole named `metrics-reader` that allows:
- get on non-resource URLs: `/metrics`, `/healthz`

---

## Question 9: Check Permissions
Use `kubectl auth can-i` to verify:
- Can ServiceAccount `app-sa` create pods in namespace `default`?
- Can user `john` delete deployments in namespace `prod`?
- Can current user list secrets in all namespaces?

---

## Question 10: Aggregate ClusterRoles
Create a ClusterRole named `monitoring` with label `rbac.example.com/aggregate-to-monitoring: "true"` that allows reading pods. Then create an aggregated ClusterRole that combines multiple monitoring roles.

---

## Question 11: Troubleshoot RBAC
A pod using ServiceAccount `app-sa` gets "forbidden" when trying to list services. Debug and fix:
- Check if ServiceAccount exists
- Verify Role/RoleBinding configuration
- Check permissions with `kubectl auth can-i`

---

## Question 12: Least Privilege
Create RBAC for a CI/CD pipeline ServiceAccount that can:
- Only create and update deployments in namespace `staging`
- Read configmaps in namespace `staging`
- Cannot delete anything

---

## Key Commands for RBAC

### Creating Resources
```bash
# Create Role
kubectl create role <role-name> --verb=<verbs> --resource=<resources> -n <namespace>

# Create ClusterRole
kubectl create clusterrole <role-name> --verb=<verbs> --resource=<resources>

# Create RoleBinding
kubectl create rolebinding <binding-name> --role=<role-name> --serviceaccount=<namespace>:<sa-name> -n <namespace>

# Create ClusterRoleBinding
kubectl create clusterrolebinding <binding-name> --clusterrole=<role-name> --user=<user>

# Create ServiceAccount
kubectl create serviceaccount <sa-name> -n <namespace>
```

### Checking Permissions
```bash
# Check if you can perform an action
kubectl auth can-i <verb> <resource>

# Check for specific namespace
kubectl auth can-i create pods -n dev

# Check as another user
kubectl auth can-i list secrets --as john@example.com

# Check as ServiceAccount
kubectl auth can-i create deployments --as system:serviceaccount:default:app-sa -n default

# List all permissions for current user
kubectl auth can-i --list
```

### Viewing Resources
```bash
# List Roles
kubectl get roles -n <namespace>

# List ClusterRoles
kubectl get clusterroles

# List RoleBindings
kubectl get rolebindings -n <namespace>

# List ClusterRoleBindings
kubectl get clusterrolebindings

# Describe Role
kubectl describe role <role-name> -n <namespace>

# Get YAML
kubectl get role <role-name> -n <namespace> -o yaml
```

---

## RBAC Concepts

### Role vs ClusterRole
- **Role**: Permissions within a specific namespace
- **ClusterRole**: Cluster-wide permissions or permissions in all namespaces

### RoleBinding vs ClusterRoleBinding
- **RoleBinding**: Grants permissions in a specific namespace (can bind Role or ClusterRole)
- **ClusterRoleBinding**: Grants cluster-wide permissions (only binds ClusterRole)

### Subjects (Who gets permissions)
- **User**: `kind: User`, `name: john@example.com`
- **Group**: `kind: Group`, `name: dev-team`
- **ServiceAccount**: `kind: ServiceAccount`, `name: app-sa`, `namespace: default`

### Verbs (Actions)
- get, list, watch (read operations)
- create, update, patch (write operations)
- delete, deletecollection (delete operations)
- * (all verbs)

### Resources
- pods, services, deployments, configmaps, secrets
- pods/log, pods/exec (subresources)
- * (all resources)

---

## Common Patterns

### Read-only access to pods
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: default
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

### Full access to deployments
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: deployment-admin
  namespace: production
rules:
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["*"]
```

### Bind to ServiceAccount
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-binding
  namespace: default
subjects:
- kind: ServiceAccount
  name: app-sa
  namespace: default
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

## Exam Tips

1. **Use imperative commands** when possible for speed
2. **Always verify with `kubectl auth can-i`** after creating RBAC
3. **Remember apiGroups**: 
   - Core resources (pods, services): `apiGroups: [""]`
   - Apps (deployments): `apiGroups: ["apps"]`
   - Networking (networkpolicies): `apiGroups: ["networking.k8s.io"]`
4. **ServiceAccount format**: `system:serviceaccount:<namespace>:<sa-name>`
5. **Test your RBAC** by creating a pod with the ServiceAccount and trying operations
6. **RoleBinding can reference ClusterRole** to grant namespace-scoped permissions

---

## Practice Strategy

1. Create ServiceAccounts
2. Create Roles with specific permissions
3. Bind them with RoleBindings
4. Verify with `kubectl auth can-i`
5. Test with actual pods using the ServiceAccount
6. Debug when permissions are denied

Good luck with your CKAD preparation!
