# CKAD Probes and Health Checks Practice Questions

## Application Observability and Maintenance (15%)

### Implement Probes and Health Checks

---

## Question 1: Basic Liveness Probe
Create a Pod named `web-app` using image `nginx` with an HTTP liveness probe that:
- Checks path `/healthz` on port 80
- Initial delay: 10 seconds
- Period: 5 seconds

---

## Question 2: Readiness Probe
Create a Deployment named `backend` with 3 replicas using image `nginx`. Add a readiness probe that:
- Checks TCP socket on port 80
- Initial delay: 5 seconds
- Period: 10 seconds

---

## Question 3: Startup Probe
Create a Pod named `slow-start` with image `nginx` that has a startup probe:
- HTTP GET on `/` port 80
- Initial delay: 0 seconds
- Period: 10 seconds
- Failure threshold: 30 (allows 300 seconds to start)

---

## Question 4: All Three Probes
Create a Pod named `full-health` with all three probe types:
- Startup probe: HTTP GET `/startup` port 8080, period 5s, failure threshold 12
- Liveness probe: HTTP GET `/health` port 8080, period 10s
- Readiness probe: HTTP GET `/ready` port 8080, period 5s

---

## Question 5: Exec Probe
Create a Pod named `file-check` with a liveness probe that:
- Uses exec command: `cat /tmp/healthy`
- Initial delay: 5 seconds
- Period: 5 seconds

---

## Question 6: Custom Headers
Create a Pod with HTTP liveness probe that includes custom HTTP headers:
- Path: `/health`
- Port: 8080
- Header: `X-Custom-Header: Awesome`

---

## Question 7: Probe Timeouts
Create a Pod with liveness probe that:
- Checks HTTP `/health` on port 80
- Timeout: 3 seconds
- Period: 10 seconds
- Failure threshold: 3

---

## Question 8: Troubleshoot Failing Probe
A Pod named `app-pod` keeps restarting. The liveness probe is failing. Debug and fix the issue.

---

## Question 9: Readiness vs Liveness
Create a Deployment where pods should not receive traffic until fully initialized, but should restart if they become unresponsive. Use appropriate probes.

---

## Question 10: Probe with Named Port
Create a Pod with container that exposes a named port `http-port: 8080`. Use this named port in the liveness probe.

---

## Question 11: Multi-Container Probes
Create a Pod with two containers. Each container should have its own liveness and readiness probes.

---

## Question 12: Update Existing Pod Probe
A Pod exists with no probes. Add a liveness probe without recreating the pod. (Hint: This requires pod restart)

---

## Question 13: Probe Delays
Create a Pod for an application that takes 60 seconds to start. Configure probes so the pod doesn't restart during startup.

---

## Question 14: Success Threshold
Create a Pod with readiness probe that requires 3 consecutive successes before marking ready.

---

## Question 15: gRPC Probe
Create a Pod with gRPC liveness probe on port 9090. (Kubernetes 1.24+)

---

## Question 16: Probe Failure Behavior
Create a Pod and observe what happens when:
- Liveness probe fails repeatedly
- Readiness probe fails repeatedly

---

## Question 17: Probe in StatefulSet
Create a StatefulSet with readiness probe to ensure orderly pod startup.

---

## Question 18: Combined Exec and HTTP
Create a Pod with:
- Liveness probe using exec command
- Readiness probe using HTTP GET

---

## Question 19: Disable Probes Temporarily
A Pod has probes causing issues during maintenance. Temporarily disable them.

---

## Question 20: Probe Best Practices
Review a Pod configuration and identify probe misconfigurations or improvements needed.

---

## Key Concepts

### Probe Types

**Liveness Probe:**
- Determines if container is running
- Fails → Container restarted
- Use: Detect deadlocks, unrecoverable states

**Readiness Probe:**
- Determines if container ready to serve traffic
- Fails → Pod removed from Service endpoints
- Use: Prevent traffic during initialization

**Startup Probe:**
- Determines if application has started
- Disables liveness/readiness until startup succeeds
- Use: Slow-starting applications

### Probe Mechanisms

**httpGet:**
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
    httpHeaders:
    - name: Custom-Header
      value: Awesome
  initialDelaySeconds: 15
  periodSeconds: 20
```

**tcpSocket:**
```yaml
readinessProbe:
  tcpSocket:
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```

**exec:**
```yaml
livenessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy
  initialDelaySeconds: 5
  periodSeconds: 5
```

**grpc:** (Kubernetes 1.24+)
```yaml
livenessProbe:
  grpc:
    port: 9090
  initialDelaySeconds: 10
```

### Probe Parameters

**initialDelaySeconds:** Wait time before first probe
**periodSeconds:** How often to probe
**timeoutSeconds:** Probe timeout
**successThreshold:** Consecutive successes needed (default: 1)
**failureThreshold:** Consecutive failures before action (default: 3)

### Probe Behavior

| Probe Type | On Failure | Affects Service | Restarts Container |
|------------|------------|-----------------|-------------------|
| Liveness | Restart container | No | Yes |
| Readiness | Remove from endpoints | Yes | No |
| Startup | Mark startup failed | No | Yes (after threshold) |

---

## Essential kubectl Commands

### View Probe Configuration
```bash
# Get pod probes
kubectl describe pod <pod-name> | grep -A 10 Liveness
kubectl describe pod <pod-name> | grep -A 10 Readiness

# Get full pod spec
kubectl get pod <pod-name> -o yaml | grep -A 20 livenessProbe
```

### Check Probe Status
```bash
# View pod conditions
kubectl get pod <pod-name> -o jsonpath='{.status.conditions[*]}'

# Check if ready
kubectl get pod <pod-name> -o jsonpath='{.status.conditions[?(@.type=="Ready")].status}'

# View container ready status
kubectl get pod <pod-name> -o jsonpath='{.status.containerStatuses[*].ready}'
```

### Debug Failed Probes
```bash
# Check pod events for probe failures
kubectl describe pod <pod-name> | grep -i probe

# View events
kubectl get events --field-selector involvedObject.name=<pod-name>

# Check restart count
kubectl get pod <pod-name>
# RESTARTS column shows liveness probe failures

# Check logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous  # Previous container instance
```

### Test Probes
```bash
# Exec into pod and test probe command
kubectl exec <pod-name> -- cat /tmp/healthy

# Test HTTP endpoint
kubectl exec <pod-name> -- wget -qO- localhost:8080/health

# Port forward and test locally
kubectl port-forward <pod-name> 8080:8080
curl http://localhost:8080/health
```

---

## YAML Examples

### HTTP Liveness Probe
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-http
spec:
  containers:
  - name: app
    image: nginx
    ports:
    - containerPort: 80
    livenessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 3
      periodSeconds: 3
```

### TCP Readiness Probe
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: readiness-tcp
spec:
  containers:
  - name: app
    image: nginx
    ports:
    - containerPort: 80
    readinessProbe:
      tcpSocket:
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
```

### Exec Liveness Probe
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-exec
spec:
  containers:
  - name: app
    image: busybox
    command: ["sh", "-c", "touch /tmp/healthy && sleep 3600"]
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```

### Startup Probe
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: startup-probe
spec:
  containers:
  - name: app
    image: nginx
    startupProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 0
      periodSeconds: 10
      failureThreshold: 30  # 30 * 10 = 300 seconds max startup time
```

### All Three Probes
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: all-probes
spec:
  containers:
  - name: app
    image: myapp:1.0
    ports:
    - containerPort: 8080
    startupProbe:
      httpGet:
        path: /startup
        port: 8080
      periodSeconds: 5
      failureThreshold: 12
    livenessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 10
    readinessProbe:
      httpGet:
        path: /ready
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 5
```

### Probe with Custom Headers
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: custom-headers
spec:
  containers:
  - name: app
    image: nginx
    livenessProbe:
      httpGet:
        path: /health
        port: 8080
        httpHeaders:
        - name: X-Custom-Header
          value: Awesome
        - name: Authorization
          value: Bearer token
      initialDelaySeconds: 3
      periodSeconds: 3
```

### Probe with Timeouts
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: timeout-probe
spec:
  containers:
  - name: app
    image: nginx
    livenessProbe:
      httpGet:
        path: /health
        port: 80
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 3
      successThreshold: 1
      failureThreshold: 3
```

### Named Port Probe
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: named-port
spec:
  containers:
  - name: app
    image: nginx
    ports:
    - name: http
      containerPort: 8080
    livenessProbe:
      httpGet:
        path: /health
        port: http  # References named port
      periodSeconds: 5
```

### Multi-Container with Probes
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container
spec:
  containers:
  - name: app
    image: nginx
    livenessProbe:
      httpGet:
        path: /
        port: 80
      periodSeconds: 5
    readinessProbe:
      httpGet:
        path: /
        port: 80
      periodSeconds: 5
  - name: sidecar
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
    livenessProbe:
      exec:
        command: ["echo", "ok"]
      periodSeconds: 10
```

### Deployment with Readiness Probe
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
        readinessProbe:
          tcpSocket:
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 10
```

### gRPC Probe (K8s 1.24+)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: grpc-probe
spec:
  containers:
  - name: app
    image: mygrpcapp:1.0
    ports:
    - containerPort: 9090
    livenessProbe:
      grpc:
        port: 9090
      initialDelaySeconds: 10
      periodSeconds: 5
```

---

## Troubleshooting Probes

### Pod Keeps Restarting

**Cause:** Liveness probe failing

```bash
# Check restart count
kubectl get pod <pod-name>
# RESTARTS: 5

# Check events
kubectl describe pod <pod-name>
# Events: Liveness probe failed: HTTP probe failed with statuscode: 404

# Check probe configuration
kubectl get pod <pod-name> -o yaml | grep -A 10 livenessProbe
```

**Fixes:**
- Increase `initialDelaySeconds` if app needs more startup time
- Fix probe path or port
- Increase `timeoutSeconds` if probe times out
- Increase `failureThreshold` to tolerate transient failures

### Pod Not Receiving Traffic

**Cause:** Readiness probe failing

```bash
# Check endpoints
kubectl get endpoints <service-name>
# ENDPOINTS: <none> or missing pod IP

# Check readiness
kubectl get pod <pod-name> -o jsonpath='{.status.conditions[?(@.type=="Ready")].status}'
# False

# Check events
kubectl describe pod <pod-name>
# Events: Readiness probe failed
```

**Fixes:**
- Check if app is actually ready
- Verify probe path/port correct
- Increase `initialDelaySeconds`
- Check app logs for errors

### Startup Takes Too Long

**Cause:** Liveness probe killing pod before startup complete

```bash
# Pod restarts during initialization
kubectl logs <pod-name> --previous
# Shows partial startup logs
```

**Fix:** Add startup probe
```yaml
startupProbe:
  httpGet:
    path: /health
    port: 8080
  periodSeconds: 10
  failureThreshold: 30  # 300 seconds total
```

### Probe Never Succeeds

```bash
# Check what probe is doing
kubectl exec <pod-name> -- wget -O- localhost:8080/health
kubectl exec <pod-name> -- cat /tmp/healthy

# Test port is listening
kubectl exec <pod-name> -- netstat -tlnp | grep 8080
```

**Common issues:**
- Wrong path or port
- App not listening on expected port
- Firewall/NetworkPolicy blocking
- App crashes before probe runs

---

## Probe Decision Matrix

| Scenario | Use | Don't Use |
|----------|-----|-----------|
| Detect deadlock | Liveness | Readiness |
| Prevent traffic during init | Readiness | Liveness |
| Slow startup (>1 min) | Startup | High initialDelay |
| Database connection required | Readiness | Liveness |
| Memory leak recovery | Liveness | Readiness |
| Rolling update | Readiness | - |

---

## Best Practices

1. **Always use readiness probes** for pods behind Services
2. **Use startup probes** for slow-starting apps (avoid high initialDelaySeconds)
3. **Liveness probes** should check application health, not dependencies
4. **Readiness probes** can check dependencies (DB, cache)
5. **Don't make probes too aggressive** - allow for transient failures
6. **Set appropriate timeouts** - network can be slow
7. **Test probe endpoints** independently
8. **Use exec probes sparingly** - they're more expensive
9. **Named ports** make configs more maintainable
10. **Monitor probe failures** in production

---

## Common Mistakes

❌ **Using liveness probe to check dependencies**
```yaml
# Bad - will restart if DB is down
livenessProbe:
  exec:
    command: ["pg_isready", "-h", "database"]
```

✅ **Use readiness for dependencies**
```yaml
# Good - removes from service if DB down
readinessProbe:
  exec:
    command: ["pg_isready", "-h", "database"]
```

❌ **Too low initialDelaySeconds**
```yaml
# Bad - app needs 30s to start
livenessProbe:
  initialDelaySeconds: 5  # Pod will restart!
```

✅ **Use startup probe**
```yaml
# Good - allows time to start
startupProbe:
  periodSeconds: 10
  failureThreshold: 30  # 300s total
```

❌ **Same probe for liveness and readiness**
```yaml
# Bad - restarts if temporarily not ready
livenessProbe:
  httpGet:
    path: /health  # Checks DB connection
```

---

## Exam Tips

1. **Know the three probe types** and when to use each
2. **Default values:** initialDelaySeconds=0, periodSeconds=10, timeoutSeconds=1, failureThreshold=3
3. **Liveness = restart**, **Readiness = remove from service**
4. **Startup probe** disables liveness/readiness until first success
5. **exec probes** use array format: `["cat", "/tmp/healthy"]`
6. **httpGet** requires path and port
7. **tcpSocket** only needs port
8. **Named ports** can be referenced in probes
9. **Quick check:** `kubectl describe pod | grep -i probe`
10. **Probe failures** shown in Events section

---

## Practice Strategy

1. Create pods with each probe type
2. Practice all three mechanisms (httpGet, tcpSocket, exec)
3. Combine probes (startup + liveness + readiness)
4. Break probes and debug (wrong path, port, timeout)
5. Test probe timing (initialDelay, period, threshold)
6. Add probes to Deployments
7. Observe behavior when probes fail
8. Practice quick probe additions to existing pods

Good luck with your CKAD preparation!
