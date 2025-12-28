# CKAD Deployments Practice Questions

## Application Deployment (20%)

### Deployments, Rolling Updates, and Rollbacks

---

## Question 1: Basic Deployment
Create a Deployment named `web-app` with:
- Image: `nginx:1.24`
- Replicas: 3
- Label: `app=web`

---

## Question 2: Update Deployment Image
A Deployment named `api-server` exists using image `nginx:1.23`. Update it to use `nginx:1.24`.

---

## Question 3: Rollback Deployment
A Deployment named `backend` was recently updated but is failing. Roll it back to the previous version.

---

## Question 4: Deployment with Resource Limits
Create a Deployment named `resource-app` with:
- Image: `nginx`
- Replicas: 2
- Resource requests: CPU 100m, Memory 128Mi
- Resource limits: CPU 200m, Memory 256Mi

---

## Question 5: Rolling Update Strategy
Create a Deployment named `rolling-app` with:
- Image: `nginx:1.23`
- Replicas: 6
- Rolling update: maxSurge 2, maxUnavailable 1

---

## Question 6: Scale Deployment
Scale the Deployment `webapp` from 3 to 5 replicas.

---

## Question 7: Deployment Rollout Status
Check the rollout status of Deployment `api-app`. Determine if the update is complete.

---

## Question 8: Deployment History
View the rollout history of Deployment `backend-app`. Identify which revision is currently active.

---

## Question 9: Rollback to Specific Revision
Deployment `frontend` has multiple revisions. Roll back to revision 3.

---

## Question 10: Pause and Resume Rollout
Pause the rollout of Deployment `staging-app`, make changes, then resume the rollout.

---

## Question 11: Record Deployment Changes
Update Deployment `api` to use image `nginx:1.25` and record the change cause as "Security patch update".

---

## Question 12: Recreate Strategy
Create a Deployment named `database-app` with:
- Image: `postgres:15`
- Replicas: 1
- Strategy: Recreate (not RollingUpdate)

---

## Question 13: Deployment with Labels
Create a Deployment with multiple labels:
- `app=web`
- `env=production`
- `tier=frontend`

---

## Question 14: Update Multiple Fields
Update Deployment `app-server`:
- Change image to `nginx:1.25`
- Change replicas to 4
- Add environment variable `ENV=production`

---

## Question 15: Zero Downtime Update
A Deployment `critical-app` must have zero downtime during updates. Configure appropriate rolling update strategy.

---

## Question 16: Deployment with Readiness Probe
Create a Deployment that ensures pods are ready before being added to Service during rolling updates.

---

## Question 17: Fast Rollback
A Deployment `web-service` is experiencing issues after an update. Perform the fastest possible rollback.

---

## Question 18: Deployment Selector
Create a Deployment that selects pods with labels `app=api` AND `version=v2`.

---

## Question 19: Update with Annotation
Update Deployment `backend` and add annotation `change-cause: "Updated to latest version"`.

---

## Question 20: Deployment Troubleshooting
A Deployment `broken-app` has 0/3 ready replicas. Debug and identify the issue.

---

## Question 21: Blue-Green Deployment Setup
Set up two Deployments (`blue` and `green`) for blue-green deployment pattern with a Service that can switch between them.

---

## Question 22: Canary Deployment
Create a canary deployment setup where 20% of traffic goes to new version and 80% to old version.

---

## Question 23: MinReadySeconds
Create a Deployment with `minReadySeconds: 30` to ensure pods are stable before proceeding with rollout.

---

## Question 24: Deployment with Anti-Affinity
Create a Deployment that spreads pods across different nodes (pod anti-affinity).

---

## Question 25: Progressive Rollout
Create a Deployment that rolls out very conservatively: only 1 pod at a time.

---

## Key Concepts

### Deployment Basics

**Deployment:**
- Manages ReplicaSet
- Provides declarative updates
- Supports rolling updates and rollbacks
- Self-healing and scaling

**ReplicaSet:**
- Created by Deployment
- Ensures desired number of pod replicas
- Usually not managed directly

### Update Strategies

**RollingUpdate (default):**
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1        # Max new pods during update
    maxUnavailable: 1  # Max unavailable pods during update
```

**Recreate:**
```yaml
strategy:
  type: Recreate  # Terminates all old pods before creating new ones
```

### Rolling Update Parameters

**maxSurge:**
- Number/percentage of pods above desired count
- Can be absolute (e.g., 2) or percentage (e.g., 25%)
- Higher = faster updates, more resources

**maxUnavailable:**
- Number/percentage of pods that can be unavailable
- Can be absolute (e.g., 1) or percentage (e.g., 25%)
- Lower = safer updates, slower

**minReadySeconds:**
- Minimum time pod should be ready without crashes
- Prevents rapid rollout of broken versions

### Deployment Lifecycle

1. **Create:** `kubectl create deployment` or `kubectl apply`
2. **Update:** Change image, replicas, or other specs
3. **Rollout:** Deployment controller creates new ReplicaSet
4. **Monitor:** `kubectl rollout status`
5. **Rollback:** `kubectl rollout undo` if issues

---

## Essential kubectl Commands

### Creating Deployments

```bash
# Imperative - basic
kubectl create deployment nginx --image=nginx

# Imperative - with replicas
kubectl create deployment web --image=nginx --replicas=3

# Generate YAML
kubectl create deployment app --image=nginx --replicas=3 --dry-run=client -o yaml > deployment.yaml

# From YAML
kubectl apply -f deployment.yaml
```

### Updating Deployments

```bash
# Update image
kubectl set image deployment/nginx nginx=nginx:1.25

# Update image and record
kubectl set image deployment/nginx nginx=nginx:1.25 --record

# Edit deployment
kubectl edit deployment nginx

# Patch deployment
kubectl patch deployment nginx -p '{"spec":{"replicas":5}}'

# Update from YAML
kubectl apply -f deployment.yaml
```

### Scaling

```bash
# Scale to 5 replicas
kubectl scale deployment nginx --replicas=5

# Autoscale (HPA)
kubectl autoscale deployment nginx --min=2 --max=10 --cpu-percent=80
```

### Rollout Management

```bash
# Check rollout status
kubectl rollout status deployment/nginx

# View rollout history
kubectl rollout history deployment/nginx

# View specific revision
kubectl rollout history deployment/nginx --revision=2

# Pause rollout
kubectl rollout pause deployment/nginx

# Resume rollout
kubectl rollout resume deployment/nginx

# Rollback to previous version
kubectl rollout undo deployment/nginx

# Rollback to specific revision
kubectl rollout undo deployment/nginx --to-revision=2

# Restart deployment (recreate all pods)
kubectl rollout restart deployment/nginx
```

### Viewing Deployments

```bash
# List deployments
kubectl get deployments
kubectl get deploy

# Describe deployment
kubectl describe deployment nginx

# Get YAML
kubectl get deployment nginx -o yaml

# Get deployment status
kubectl get deployment nginx -o wide

# Watch deployment
kubectl get deployment nginx -w

# Get ReplicaSets
kubectl get rs
```

### Deployment Information

```bash
# Get deployment selector
kubectl get deployment nginx -o jsonpath='{.spec.selector}'

# Get current replicas
kubectl get deployment nginx -o jsonpath='{.status.replicas}'

# Get ready replicas
kubectl get deployment nginx -o jsonpath='{.status.readyReplicas}'

# Get strategy
kubectl get deployment nginx -o jsonpath='{.spec.strategy}'

# Get revision
kubectl get deployment nginx -o jsonpath='{.metadata.annotations.deployment\.kubernetes\.io/revision}'
```

---

## YAML Examples

### Basic Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```

### Deployment with Resource Limits

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: resource-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: resource
  template:
    metadata:
      labels:
        app: resource
    spec:
      containers:
      - name: nginx
        image: nginx
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "200m"
```

### RollingUpdate Strategy

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rolling-app
spec:
  replicas: 6
  selector:
    matchLabels:
      app: rolling
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 2          # 2 extra pods during update
      maxUnavailable: 1    # Only 1 pod can be unavailable
  template:
    metadata:
      labels:
        app: rolling
    spec:
      containers:
      - name: nginx
        image: nginx:1.23
```

### Recreate Strategy

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: database-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: database
  strategy:
    type: Recreate  # Kills all old pods before creating new
  template:
    metadata:
      labels:
        app: database
    spec:
      containers:
      - name: postgres
        image: postgres:15
```

### Deployment with MinReadySeconds

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: stable-app
spec:
  replicas: 3
  minReadySeconds: 30  # Wait 30s before considering pod ready
  selector:
    matchLabels:
      app: stable
  template:
    metadata:
      labels:
        app: stable
    spec:
      containers:
      - name: nginx
        image: nginx
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 5
```

### Zero Downtime Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: critical-app
spec:
  replicas: 5
  selector:
    matchLabels:
      app: critical
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 2
      maxUnavailable: 0  # Never have unavailable pods
  template:
    metadata:
      labels:
        app: critical
    spec:
      containers:
      - name: app
        image: myapp:1.0
        readinessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
```

### Deployment with Multiple Labels

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: multi-label-app
  labels:
    app: web
    env: production
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
      env: production
      tier: frontend
  template:
    metadata:
      labels:
        app: web
        env: production
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```

### Deployment with Environment Variables

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: env-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: env
  template:
    metadata:
      labels:
        app: env
    spec:
      containers:
      - name: app
        image: myapp:1.0
        env:
        - name: ENV
          value: "production"
        - name: DEBUG
          value: "false"
        - name: DATABASE_URL
          value: "postgres://db:5432"
```

### Conservative Rollout (One at a Time)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: conservative-app
spec:
  replicas: 5
  selector:
    matchLabels:
      app: conservative
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  minReadySeconds: 60  # Wait 60s between pods
  template:
    metadata:
      labels:
        app: conservative
    spec:
      containers:
      - name: app
        image: myapp:1.0
```

---

## Troubleshooting Deployments

### Deployment Not Rolling Out

```bash
# Check rollout status
kubectl rollout status deployment/myapp

# Check events
kubectl get events --sort-by='.lastTimestamp' | grep myapp

# Check ReplicaSets
kubectl get rs -l app=myapp

# Describe deployment
kubectl describe deployment myapp
```

**Common causes:**
- Image pull errors
- Insufficient resources
- Failing readiness probes
- Invalid configuration

### Pods Not Ready

```bash
# Check pod status
kubectl get pods -l app=myapp

# Describe pods
kubectl describe pod <pod-name>

# Check logs
kubectl logs <pod-name>

# Check previous logs (if restarting)
kubectl logs <pod-name> --previous
```

### Rollout Stuck

```bash
# Check rollout status
kubectl rollout status deployment/myapp
# "Waiting for deployment spec update to be observed..."

# Check if paused
kubectl get deployment myapp -o jsonpath='{.spec.paused}'

# Resume if paused
kubectl rollout resume deployment/myapp

# Check for blocking issues
kubectl describe deployment myapp
```

### Image Update Not Working

```bash
# Verify image was updated
kubectl get deployment myapp -o jsonpath='{.spec.template.spec.containers[0].image}'

# Check if new ReplicaSet created
kubectl get rs -l app=myapp --sort-by='.metadata.creationTimestamp'

# Force update
kubectl rollout restart deployment/myapp
```

---

## Blue-Green Deployment Pattern

### Setup

```yaml
# Blue Deployment (current)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      version: blue
  template:
    metadata:
      labels:
        app: myapp
        version: blue
    spec:
      containers:
      - name: app
        image: myapp:1.0
---
# Green Deployment (new)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-green
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      version: green
  template:
    metadata:
      labels:
        app: myapp
        version: green
    spec:
      containers:
      - name: app
        image: myapp:2.0
---
# Service (points to blue)
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
    version: blue  # Switch to 'green' to cutover
  ports:
  - port: 80
```

### Switch Traffic

```bash
# Switch from blue to green
kubectl patch service myapp -p '{"spec":{"selector":{"version":"green"}}}'

# Rollback to blue if needed
kubectl patch service myapp -p '{"spec":{"selector":{"version":"blue"}}}'
```

---

## Canary Deployment Pattern

### Setup (80% old, 20% new)

```yaml
# Stable Deployment (80%)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-stable
spec:
  replicas: 4  # 80% of 5 total
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
        version: stable
    spec:
      containers:
      - name: app
        image: myapp:1.0
---
# Canary Deployment (20%)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-canary
spec:
  replicas: 1  # 20% of 5 total
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
        version: canary
    spec:
      containers:
      - name: app
        image: myapp:2.0
---
# Service (selects both)
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp  # Selects both stable and canary
  ports:
  - port: 80
```

---

## Common Mistakes

### ❌ Selector Doesn't Match Labels

```yaml
# Wrong
spec:
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: api  # Doesn't match!
```

### ❌ maxUnavailable and maxSurge Both 0

```yaml
# Wrong - deployment can't proceed
rollingUpdate:
  maxSurge: 0
  maxUnavailable: 0
```

### ❌ Recreate with Multiple Replicas

```yaml
# Bad practice - causes downtime
spec:
  replicas: 5
  strategy:
    type: Recreate  # All 5 pods killed at once!
```

### ❌ No Readiness Probe in Rolling Update

```yaml
# Risky - pods added to service before ready
spec:
  template:
    spec:
      containers:
      - name: app
        image: myapp
        # Missing readinessProbe!
```

---

## Best Practices

1. **Always use readiness probes** with rolling updates
2. **Set appropriate maxSurge/maxUnavailable** for your app
3. **Use minReadySeconds** to prevent rapid broken rollouts
4. **Record changes** with `--record` flag
5. **Test rollbacks** in non-production first
6. **Monitor rollout status** during updates
7. **Set resource limits** to prevent pod evictions during updates
8. **Use labels** for organization and selection
9. **Start conservative** (low maxSurge/maxUnavailable), increase if safe
10. **Zero maxUnavailable** for critical apps

---

## Exam Tips

1. **Quick deployment creation:** `kubectl create deployment`
2. **Update image:** `kubectl set image deployment/name container=image`
3. **Rollback:** `kubectl rollout undo deployment/name`
4. **Check status:** `kubectl rollout status deployment/name`
5. **Scale:** `kubectl scale deployment/name --replicas=N`
6. **Selector MUST match** template labels exactly
7. **Default strategy:** RollingUpdate with maxSurge=25%, maxUnavailable=25%
8. **Record flag:** `--record` (deprecated but still works)
9. **Generate YAML:** `--dry-run=client -o yaml`
10. **Watch rollout:** `kubectl get deployment -w`

---

## Practice Strategy

1. Create deployments imperatively and declaratively
2. Practice all update methods (set image, edit, apply)
3. Roll out updates and monitor status
4. Practice rollbacks (undo, to specific revision)
5. Experiment with different strategies
6. Break deployments and troubleshoot
7. Practice blue-green and canary patterns
8. Time yourself - deployments are common on exam

Good luck with your CKAD preparation!
