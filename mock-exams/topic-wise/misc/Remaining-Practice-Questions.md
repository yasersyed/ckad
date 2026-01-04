# CKAD Remaining Topics Practice Questions

## Commands and Arguments (10 Questions)

### Question 1: Override Command
Create a Pod named `custom-cmd` with image `busybox` that runs the command `echo "Hello CKAD"`.

---

### Question 2: Override Arguments
Create a Pod named `custom-args` with image `busybox` that overrides arguments to `sleep 3600`.

---

### Question 3: Command and Args Together
Create a Pod named `cmd-args` with image `busybox` using:
- command: `sh -c`
- args: `echo "Start" && sleep 3600`

---

### Question 4: Multiple Arguments
Create a Pod with image `nginx` that runs: `nginx -g "daemon off;"` using command and args.

---

### Question 5: Environment Variable in Command
Create a Pod that uses environment variable `NAME=World` in command: `echo "Hello $NAME"`.

---

### Question 6: Override Dockerfile ENTRYPOINT
A Dockerfile has `ENTRYPOINT ["python", "app.py"]`. Create a Pod that overrides this to run `python debug.py`.

---

### Question 7: Override Dockerfile CMD
A Dockerfile has `CMD ["start"]`. Create a Pod that overrides this to use `["debug"]`.

---

### Question 8: Array vs String Format
Fix this broken Pod that uses wrong command format:
```yaml
command: "echo hello"  # Wrong format
```

---

### Question 9: Multi-line Command
Create a Pod that runs a multi-line bash script:
```bash
echo "Starting..."
sleep 10
echo "Done"
```

---

### Question 10: Troubleshoot Command Error
A Pod is in CrashLoopBackOff. The command is set incorrectly. Debug and fix it.

---

## Resource Requests and Limits (10 Questions)

### Question 11: Basic Requests
Create a Pod named `resource-pod` with:
- CPU request: 100m
- Memory request: 128Mi

---

### Question 12: Requests and Limits
Create a Pod with:
- CPU request: 100m, limit: 200m
- Memory request: 128Mi, limit: 256Mi

---

### Question 13: Update Deployment Resources
A Deployment `web-app` exists. Update it to set CPU request to 200m and memory limit to 512Mi.

---

### Question 14: Pending Pod - Insufficient Resources
A Pod is Pending due to insufficient CPU. Reduce its CPU request from 4 to 500m.

---

### Question 15: OOMKilled Pod
A Pod keeps getting OOMKilled. Current memory limit is 128Mi. Increase it to 512Mi.

---

### Question 16: Multiple Containers Resources
Create a Pod with two containers, each with different resource requirements:
- nginx: CPU 100m, Memory 128Mi
- sidecar: CPU 50m, Memory 64Mi

---

### Question 17: CPU in Millicores
Set CPU request to 0.5 cores using millicores notation.

---

### Question 18: Memory Units
Create a Pod with memory request of 1 Gibibyte.

---

### Question 19: View Resource Usage
Check current CPU and memory usage of all pods in namespace `production`.

---

### Question 20: No Limits
Create a Pod with resource requests but NO limits.

---

## ServiceAccounts and RBAC (10 Questions)

### Question 21: Create ServiceAccount
Create a ServiceAccount named `app-sa` in namespace `default`.

---

### Question 22: Pod with ServiceAccount
Create a Pod named `auth-pod` that uses ServiceAccount `app-sa`.

---

### Question 23: Create Role
Create a Role named `pod-reader` that allows GET, LIST, WATCH on pods.

---

### Question 24: Create RoleBinding
Create a RoleBinding named `read-pods` that binds Role `pod-reader` to ServiceAccount `app-sa`.

---

### Question 25: ClusterRole
Create a ClusterRole named `secret-reader` that allows GET on secrets cluster-wide.

---

### Question 26: ClusterRoleBinding
Create a ClusterRoleBinding that binds ClusterRole `secret-reader` to user `john`.

---

### Question 27: List Permissions
Check what permissions ServiceAccount `app-sa` has in the current namespace.

---

### Question 28: ServiceAccount Token
Get the token from ServiceAccount `app-sa` and decode it.

---

### Question 29: Update Deployment ServiceAccount
A Deployment `web-app` exists. Update it to use ServiceAccount `custom-sa`.

---

### Question 30: Troubleshoot RBAC
A Pod cannot list pods. Create appropriate Role and RoleBinding to fix it.

---

## ResourceQuotas and LimitRanges (10 Questions)

### Question 31: Create ResourceQuota
Create a ResourceQuota in namespace `dev` that limits:
- Total pods: 10
- Total CPU requests: 4 cores
- Total memory requests: 8Gi

---

### Question 32: Pod Limit Quota
Create a ResourceQuota that limits maximum 5 pods in namespace `test`.

---

### Question 33: Check Quota Usage
View the current ResourceQuota usage in namespace `production`.

---

### Question 34: Create LimitRange
Create a LimitRange in namespace `dev` with:
- Default CPU request: 100m
- Default memory request: 128Mi

---

### Question 35: LimitRange Min/Max
Create a LimitRange with:
- Min CPU: 50m
- Max CPU: 2 cores
- Min memory: 64Mi
- Max memory: 1Gi

---

### Question 36: Pod Exceeds Quota
A Pod creation fails with "exceeded quota". Identify which quota is exceeded and fix it.

---

### Question 37: Default Limits
Create a LimitRange that sets default CPU limit to 500m and memory limit to 512Mi.

---

### Question 38: Quota for Secrets
Create a ResourceQuota limiting maximum 5 secrets in namespace.

---

### Question 39: Multiple ResourceQuotas
Check if multiple ResourceQuotas can exist in same namespace.

---

### Question 40: LimitRange Ratios
Create a LimitRange with max limit/request ratio of 2 for memory.

---

## Labels and Selectors Advanced (10 Questions)

### Question 41: Set-Based Selector
Create a Deployment that selects pods with label `env in (dev, staging)`.

---

### Question 42: Equality and Set-Based
Create a Service that selects pods with:
- `app=web` (equality)
- `tier in (frontend, backend)` (set-based)

---

### Question 43: NotIn Operator
Create a NetworkPolicy that selects pods with `env notin (prod)`.

---

### Question 44: Exists Operator
Select all pods that have label key `version` regardless of value.

---

### Question 45: DoesNotExist Operator
Select all pods that do NOT have label key `legacy`.

---

### Question 46: Multiple Label Filters
Get pods with `app=web` AND `env=prod` AND `tier=frontend`.

---

### Question 47: Label a Running Pod
Add label `version=v2` to running pod `web-app-123`.

---

### Question 48: Remove Label
Remove label `old-label` from pod `app-pod`.

---

### Question 49: Overwrite Label
Change label `env=dev` to `env=prod` on pod `test-pod`.

---

### Question 50: Annotations vs Labels
Add annotation `description="Main web application"` to deployment `web-app`.

---

## Container Lifecycle Hooks (10 Questions)

### Question 51: postStart Hook
Create a Pod with postStart hook that creates file `/tmp/started`.

---

### Question 52: preStop Hook
Create a Pod with preStop hook that runs `nginx -s quit` for graceful shutdown.

---

### Question 53: Both Hooks
Create a Pod with both postStart and preStop hooks.

---

### Question 54: postStart with Command
Create a Pod with postStart hook that runs: `echo "Pod started" > /var/log/start.log`.

---

### Question 55: preStop Delay
Create a Pod with preStop hook that sleeps 30 seconds before termination.

---

### Question 56: postStart HTTP
Create a Pod with postStart httpGet hook to `http://localhost/health`.

---

### Question 57: Lifecycle Hook Failure
A Pod fails to start due to postStart hook failure. Debug and fix it.

---

### Question 58: Graceful Shutdown
Create a Pod that uses preStop to ensure 30-second graceful shutdown period.

---

### Question 59: postStart Exec
Create a Pod with postStart exec hook running multiple commands.

---

### Question 60: terminationGracePeriodSeconds
Create a Pod with terminationGracePeriodSeconds set to 60 and appropriate preStop hook.

---