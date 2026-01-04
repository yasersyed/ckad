# CKAD Environment Variables Practice Questions

## Application Environment, Configuration and Security (25%)

### Define resource requirements and environment variables

---

## Question 1: Simple Environment Variable
Create a Pod named `env-pod` with image `nginx` that has an environment variable `APP_ENV=production`.

---

## Question 2: Multiple Environment Variables
Create a Pod named `multi-env` with image `busybox` that has:
- `DB_HOST=localhost`
- `DB_PORT=5432`
- `DEBUG=true`
Command: `sleep 3600`

---

## Question 3: Environment from ConfigMap (All Keys)
A ConfigMap named `app-config` exists. Create a Pod named `config-pod` that loads all keys from this ConfigMap as environment variables.

---

## Question 4: Specific Key from ConfigMap
A ConfigMap named `db-config` has keys `host`, `port`, and `name`. Create a Pod that only uses the `host` key as environment variable `DB_HOST`.

---

## Question 5: Environment from Secret (All Keys)
A Secret named `db-secret` exists. Create a Pod named `secret-pod` that loads all keys from this Secret as environment variables.

---

## Question 6: Specific Key from Secret
A Secret named `api-creds` has keys `username` and `password`. Create a Pod that uses:
- `username` → `API_USER`
- `password` → `API_PASS`

---

## Question 7: Mix ConfigMap and Secret
Create a Pod that has environment variables from:
- ConfigMap `app-config` (all keys)
- Secret `app-secret` (all keys)
- Regular env var: `ENV=production`

---

## Question 8: Environment with Prefix
Load all keys from ConfigMap `db-config` with prefix `DB_`.

---

## Question 9: Field Reference (Pod Info)
Create a Pod with environment variables that reference:
- Pod name → `MY_POD_NAME`
- Pod namespace → `MY_POD_NAMESPACE`
- Pod IP → `MY_POD_IP`

---

## Question 10: Resource Limits as Environment
Create a Pod with environment variables that reference:
- CPU limit → `MY_CPU_LIMIT`
- Memory limit → `MY_MEM_LIMIT`

---

## Question 11: Update Deployment Environment
A Deployment named `web-app` exists. Add environment variable `VERSION=2.0` to it.

---

## Question 12: Remove Environment Variable
A Deployment has environment variable `OLD_VAR`. Remove it.

---

## Question 13: List Environment Variables
Check what environment variables are set in a running Pod named `app-pod`.

---

## Question 14: ConfigMap from Literals
Create a ConfigMap named `app-settings` with:
- `log_level=debug`
- `api_url=https://api.example.com`
Then create a Pod that uses it.

---

## Question 15: Secret from Literals
Create a Secret named `db-password` with key `password=secretpass`. Create a Pod that uses it as `DB_PASSWORD` environment variable.

---

## Question 16: Troubleshoot Missing ConfigMap
A Pod fails to start with "configmap not found". Debug and fix the issue.

---

## Question 17: Environment Variable in Command
Create a Pod that uses environment variable in its command: `echo "Environment: $APP_ENV"`.

---

## Question 18: Override ConfigMap Value
A Pod loads environment from ConfigMap, but one value needs to be overridden. How to do it?

---

## Question 19: Deployment with ConfigMap
Create a Deployment with 3 replicas that loads environment from ConfigMap `web-config`.

---

## Question 20: Update ConfigMap
Update a ConfigMap and verify the change appears in pods (or explain why it doesn't).

---

## Key Concepts

### Environment Variable Sources

**1. Direct (literal values):**
```yaml
env:
- name: APP_ENV
  value: production
```

**2. From ConfigMap (all keys):**
```yaml
envFrom:
- configMapRef:
    name: app-config
```

**3. From Secret (all keys):**
```yaml
envFrom:
- secretRef:
    name: app-secret
```

**4. Specific key from ConfigMap:**
```yaml
env:
- name: DB_HOST
  valueFrom:
    configMapKeyRef:
      name: db-config
      key: host
```

**5. Specific key from Secret:**
```yaml
env:
- name: PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

**6. Field reference:**
```yaml
env:
- name: MY_POD_NAME
  valueFrom:
    fieldRef:
      fieldPath: metadata.name
```

**7. Resource reference:**
```yaml
env:
- name: MY_CPU_LIMIT
  valueFrom:
    resourceFieldRef:
      containerName: myapp
      resource: limits.cpu
```

---

## Essential kubectl Commands

### Set Environment Variables

```bash
# Add env var to deployment
kubectl set env deployment/web APP_ENV=production

# Add multiple env vars
kubectl set env deployment/web ENV=prod DEBUG=false

# From ConfigMap (all keys)
kubectl set env deployment/web --from=configmap/app-config

# From Secret (all keys)
kubectl set env deployment/web --from=secret/app-secret

# Specific key from ConfigMap
kubectl set env deployment/web DB_HOST --from=configmap/db-config --keys=host

# Remove env var (use -)
kubectl set env deployment/web OLD_VAR-

# For specific container
kubectl set env deployment/web ENV=prod -c=nginx
```

### List Environment Variables

```bash
# List env vars in deployment
kubectl set env deployment/web --list

# Check in running pod
kubectl exec pod-name -- env
kubectl exec pod-name -- printenv

# Get specific var
kubectl exec pod-name -- echo $DB_HOST
```

### Create ConfigMap

```bash
# From literals
kubectl create configmap app-config \
  --from-literal=DB_HOST=localhost \
  --from-literal=DB_PORT=5432

# From file
kubectl create configmap app-config --from-file=config.properties

# From env file
kubectl create configmap app-config --from-env-file=app.env
```

### Create Secret

```bash
# From literals
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=secret

# From file
kubectl create secret generic tls-secret \
  --from-file=tls.crt \
  --from-file=tls.key
```

---

## YAML Examples

### Simple Environment Variables

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: env-pod
spec:
  containers:
  - name: app
    image: nginx
    env:
    - name: APP_ENV
      value: production
    - name: DEBUG
      value: "true"
    - name: PORT
      value: "8080"
```

### From ConfigMap (All Keys)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-pod
spec:
  containers:
  - name: app
    image: nginx
    envFrom:
    - configMapRef:
        name: app-config
```

### From Secret (All Keys)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
spec:
  containers:
  - name: app
    image: nginx
    envFrom:
    - secretRef:
        name: app-secret
```

### Specific Key from ConfigMap

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: specific-config
spec:
  containers:
  - name: app
    image: nginx
    env:
    - name: DB_HOST
      valueFrom:
        configMapKeyRef:
          name: db-config
          key: host
    - name: DB_PORT
      valueFrom:
        configMapKeyRef:
          name: db-config
          key: port
```

### Specific Key from Secret

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: specific-secret
spec:
  containers:
  - name: app
    image: nginx
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: username
    - name: DB_PASS
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
```

### Mix ConfigMap, Secret, and Literal

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mixed-env
spec:
  containers:
  - name: app
    image: nginx
    env:
    - name: APP_ENV
      value: production
    - name: DB_HOST
      valueFrom:
        configMapKeyRef:
          name: db-config
          key: host
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
    envFrom:
    - configMapRef:
        name: app-config
      prefix: APP_
```

### With Prefix

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: prefix-pod
spec:
  containers:
  - name: app
    image: nginx
    envFrom:
    - configMapRef:
        name: db-config
      prefix: DB_
    - secretRef:
        name: api-creds
      prefix: API_
```

### Field References

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: field-ref-pod
spec:
  containers:
  - name: app
    image: nginx
    env:
    - name: MY_POD_NAME
      valueFrom:
        fieldRef:
          fieldPath: metadata.name
    - name: MY_POD_NAMESPACE
      valueFrom:
        fieldRef:
          fieldPath: metadata.namespace
    - name: MY_POD_IP
      valueFrom:
        fieldRef:
          fieldPath: status.podIP
    - name: MY_NODE_NAME
      valueFrom:
        fieldRef:
          fieldPath: spec.nodeName
```

### Resource Field References

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-ref-pod
spec:
  containers:
  - name: app
    image: nginx
    resources:
      requests:
        memory: "128Mi"
        cpu: "100m"
      limits:
        memory: "256Mi"
        cpu: "200m"
    env:
    - name: MY_CPU_REQUEST
      valueFrom:
        resourceFieldRef:
          containerName: app
          resource: requests.cpu
    - name: MY_MEM_LIMIT
      valueFrom:
        resourceFieldRef:
          containerName: app
          resource: limits.memory
```

### Environment in Command

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: env-command
spec:
  containers:
  - name: app
    image: busybox
    env:
    - name: GREETING
      value: "Hello"
    - name: NAME
      value: "World"
    command:
    - sh
    - -c
    - echo "$GREETING $NAME"
```

### Deployment with Environment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx
        env:
        - name: ENV
          value: production
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: db-config
              key: host
        envFrom:
        - configMapRef:
            name: app-config
```

---

## Available Field Paths

### metadata.*
- `metadata.name` - Pod name
- `metadata.namespace` - Pod namespace
- `metadata.uid` - Pod UID
- `metadata.labels['<KEY>']` - Specific label
- `metadata.annotations['<KEY>']` - Specific annotation

### spec.*
- `spec.nodeName` - Node name
- `spec.serviceAccountName` - ServiceAccount name

### status.*
- `status.podIP` - Pod IP address
- `status.hostIP` - Node IP address

---

## Resource Field Paths

- `requests.cpu`
- `requests.memory`
- `requests.ephemeral-storage`
- `limits.cpu`
- `limits.memory`
- `limits.ephemeral-storage`

---

## Troubleshooting

### ConfigMap Not Found

```bash
# Error: configmap "app-config" not found

# Check if ConfigMap exists
kubectl get configmap app-config

# Create if missing
kubectl create configmap app-config \
  --from-literal=key=value

# Check pod events
kubectl describe pod failing-pod
```

### Secret Not Found

```bash
# Error: secret "db-secret" not found

# Check if Secret exists
kubectl get secret db-secret

# Create if missing
kubectl create secret generic db-secret \
  --from-literal=password=secret
```

### Environment Variable Not Set

```bash
# Check env vars in running pod
kubectl exec pod-name -- env | grep VAR_NAME

# If not set, check pod spec
kubectl get pod pod-name -o yaml | grep -A 20 env
```

### ConfigMap Key Not Found

```bash
# Error: key "host" not found in ConfigMap "db-config"

# Check ConfigMap keys
kubectl get configmap db-config -o yaml

# Add missing key
kubectl edit configmap db-config
```

### Changes Not Reflected

**Important:** Pods do NOT automatically reload when ConfigMap/Secret changes.

```bash
# Update ConfigMap
kubectl edit configmap app-config

# Pods won't see changes until restarted
kubectl rollout restart deployment/web-app

# Or delete pods manually
kubectl delete pod -l app=web
```

---

## Common Patterns

### Database Connection

```yaml
env:
- name: DATABASE_URL
  value: postgresql://$(DB_USER):$(DB_PASS)@$(DB_HOST):$(DB_PORT)/$(DB_NAME)
- name: DB_HOST
  valueFrom:
    configMapKeyRef:
      name: db-config
      key: host
- name: DB_PORT
  valueFrom:
    configMapKeyRef:
      name: db-config
      key: port
- name: DB_USER
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: username
- name: DB_PASS
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

### Feature Flags

```yaml
envFrom:
- configMapRef:
    name: feature-flags
    # Contains: FEATURE_X=true, FEATURE_Y=false
```

### Multi-Environment

```yaml
# dev-config ConfigMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  ENV: development
  API_URL: https://dev-api.example.com
  DEBUG: "true"

---
# prod-config ConfigMap  
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  ENV: production
  API_URL: https://api.example.com
  DEBUG: "false"
```

---

## Best Practices

1. **Use ConfigMaps for non-sensitive data**
2. **Use Secrets for sensitive data** (passwords, tokens)
3. **Use envFrom for multiple variables** from same source
4. **Use env with valueFrom for specific keys**
5. **Name environment variables in UPPER_CASE**
6. **Document required environment variables**
7. **Provide defaults in application code**
8. **Don't hardcode in Dockerfiles** - use env vars
9. **Restart pods after ConfigMap/Secret changes**
10. **Use prefixes** to avoid naming conflicts

---

## Exam Tips

1. **Know all env sources:** literal, ConfigMap, Secret, fieldRef, resourceFieldRef
2. **envFrom vs env:** All keys vs specific keys
3. **Quick set:** `kubectl set env deployment/name KEY=value`
4. **List vars:** `kubectl set env deployment/name --list`
5. **From ConfigMap:** `kubectl set env deployment/name --from=configmap/name`
6. **Check in pod:** `kubectl exec pod -- env`
7. **Field paths:** Know `metadata.name`, `status.podIP`
8. **Pods don't auto-reload** ConfigMap/Secret changes
9. **Prefix syntax:** `envFrom` with `prefix: DB_`
10. **Remove var:** `kubectl set env deployment/name VAR-`

---

## Practice Strategy

1. Create pods with literal env vars
2. Create ConfigMaps and load as env
3. Create Secrets and load as env
4. Practice mixing sources
5. Use field references
6. Use resource references
7. Update deployments with kubectl set env
8. Troubleshoot missing ConfigMaps/Secrets
9. Practice with prefixes
10. Use env vars in commands

Good luck with your CKAD preparation!