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

## Answer Key Summary

### Commands and Arguments

**Q1:**
```yaml
command: ["echo", "Hello CKAD"]
```

**Q2:**
```yaml
args: ["sleep", "3600"]
```

**Q3:**
```yaml
command: ["sh", "-c"]
args: ["echo 'Start' && sleep 3600"]
```

**Q4:**
```yaml
command: ["nginx"]
args: ["-g", "daemon off;"]
```

**Q5:**
```yaml
env:
- name: NAME
  value: World
command: ["sh", "-c", "echo Hello $NAME"]
```

**Q6:**
```yaml
command: ["python", "debug.py"]
```

**Q7:**
```yaml
args: ["debug"]
```

**Q8:**
```yaml
# Wrong
command: "echo hello"

# Correct
command: ["echo", "hello"]
# Or
command: ["/bin/sh", "-c", "echo hello"]
```

**Q9:**
```yaml
command:
- sh
- -c
- |
  echo "Starting..."
  sleep 10
  echo "Done"
```

**Q10:**
```bash
kubectl logs pod-name --previous
kubectl describe pod pod-name
# Fix command array format
```

---

### Resource Requests and Limits

**Q11:**
```yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi
```

**Q12:**
```yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 200m
    memory: 256Mi
```

**Q13:**
```bash
kubectl set resources deployment web-app --requests=cpu=200m --limits=memory=512Mi
```

**Q14:**
```bash
kubectl edit deployment name
# Change requests.cpu from 4 to 500m
```

**Q15:**
```bash
kubectl set resources deployment name --limits=memory=512Mi
```

**Q16:**
```yaml
containers:
- name: nginx
  resources:
    requests:
      cpu: 100m
      memory: 128Mi
- name: sidecar
  resources:
    requests:
      cpu: 50m
      memory: 64Mi
```

**Q17:**
```yaml
resources:
  requests:
    cpu: 500m  # 0.5 cores
```

**Q18:**
```yaml
resources:
  requests:
    memory: 1Gi
```

**Q19:**
```bash
kubectl top pods -n production
```

**Q20:**
```yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi
  # No limits section
```

---

### ServiceAccounts and RBAC

**Q21:**
```bash
kubectl create serviceaccount app-sa
```

**Q22:**
```yaml
spec:
  serviceAccountName: app-sa
```

**Q23:**
```bash
kubectl create role pod-reader --verb=get,list,watch --resource=pods
```

**Q24:**
```bash
kubectl create rolebinding read-pods --role=pod-reader --serviceaccount=default:app-sa
```

**Q25:**
```bash
kubectl create clusterrole secret-reader --verb=get --resource=secrets
```

**Q26:**
```bash
kubectl create clusterrolebinding bind-secret-reader --clusterrole=secret-reader --user=john
```

**Q27:**
```bash
kubectl auth can-i --list --as=system:serviceaccount:default:app-sa
```

**Q28:**
```bash
kubectl get secret $(kubectl get sa app-sa -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}' | base64 -d
```

**Q29:**
```bash
kubectl set serviceaccount deployment web-app custom-sa
```

**Q30:**
```bash
kubectl create role pod-lister --verb=list --resource=pods
kubectl create rolebinding bind-pod-lister --role=pod-lister --serviceaccount=default:default
```

---

### ResourceQuotas and LimitRanges

**Q31:**
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 8Gi
```

**Q32:**
```yaml
spec:
  hard:
    pods: "5"
```

**Q33:**
```bash
kubectl describe resourcequota -n production
```

**Q34:**
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: dev-limits
  namespace: dev
spec:
  limits:
  - default:
      cpu: 100m
      memory: 128Mi
    type: Container
```

**Q35:**
```yaml
spec:
  limits:
  - min:
      cpu: 50m
      memory: 64Mi
    max:
      cpu: 2
      memory: 1Gi
    type: Container
```

**Q36:**
```bash
kubectl describe resourcequota -n namespace
# Identify exceeded quota
# Reduce pod resources or increase quota
```

**Q37:**
```yaml
spec:
  limits:
  - default:
      cpu: 500m
      memory: 512Mi
    type: Container
```

**Q38:**
```yaml
spec:
  hard:
    secrets: "5"
```

**Q39:**
Yes, multiple ResourceQuotas can exist. They are cumulative.

**Q40:**
```yaml
spec:
  limits:
  - maxLimitRequestRatio:
      memory: 2
    type: Container
```

---

### Labels and Selectors Advanced

**Q41:**
```yaml
spec:
  selector:
    matchExpressions:
    - key: env
      operator: In
      values:
      - dev
      - staging
```

**Q42:**
Services only support equality-based selectors in spec.selector:
```yaml
spec:
  selector:
    app: web
```

**Q43:**
```yaml
spec:
  podSelector:
    matchExpressions:
    - key: env
      operator: NotIn
      values:
      - prod
```

**Q44:**
```bash
kubectl get pods -l version
# Or in matchExpressions:
- key: version
  operator: Exists
```

**Q45:**
```bash
kubectl get pods -l '!legacy'
# Or in matchExpressions:
- key: legacy
  operator: DoesNotExist
```

**Q46:**
```bash
kubectl get pods -l app=web,env=prod,tier=frontend
```

**Q47:**
```bash
kubectl label pod web-app-123 version=v2
```

**Q48:**
```bash
kubectl label pod app-pod old-label-
```

**Q49:**
```bash
kubectl label pod test-pod env=prod --overwrite
```

**Q50:**
```bash
kubectl annotate deployment web-app description="Main web application"
```

---

### Container Lifecycle Hooks

**Q51:**
```yaml
spec:
  containers:
  - name: app
    lifecycle:
      postStart:
        exec:
          command:
          - touch
          - /tmp/started
```

**Q52:**
```yaml
lifecycle:
  preStop:
    exec:
      command:
      - nginx
      - -s
      - quit
```

**Q53:**
```yaml
lifecycle:
  postStart:
    exec:
      command: ["/bin/sh", "-c", "echo started > /tmp/started"]
  preStop:
    exec:
      command: ["/bin/sh", "-c", "sleep 30"]
```

**Q54:**
```yaml
lifecycle:
  postStart:
    exec:
      command:
      - sh
      - -c
      - echo "Pod started" > /var/log/start.log
```

**Q55:**
```yaml
lifecycle:
  preStop:
    exec:
      command:
      - sleep
      - "30"
```

**Q56:**
```yaml
lifecycle:
  postStart:
    httpGet:
      path: /health
      port: 80
```

**Q57:**
```bash
kubectl describe pod name
# Check postStart hook error
# Fix hook command
```

**Q58:**
```yaml
spec:
  terminationGracePeriodSeconds: 30
  containers:
  - lifecycle:
      preStop:
        exec:
          command: ["/bin/sh", "-c", "sleep 30"]
```

**Q59:**
```yaml
lifecycle:
  postStart:
    exec:
      command:
      - sh
      - -c
      - |
        echo "Starting"
        mkdir -p /var/log
        date > /var/log/startup.log
```

**Q60:**
```yaml
spec:
  terminationGracePeriodSeconds: 60
  containers:
  - lifecycle:
      preStop:
        exec:
          command: ["/bin/sh", "-c", "sleep 60"]
```

---

## Key Concepts Reference

### Commands and Arguments

**Docker:**
- `ENTRYPOINT` → Kubernetes `command`
- `CMD` → Kubernetes `args`

**Format:**
- Array: `["cmd", "arg1", "arg2"]` ✅
- String: `"cmd arg1 arg2"` ❌ (doesn't work)

**Override:**
- `command` overrides `ENTRYPOINT`
- `args` overrides `CMD`
- Both can be used together

---

### Resource Units

**CPU:**
- `1` = 1 core
- `100m` = 0.1 core (100 millicores)
- `500m` = 0.5 core

**Memory:**
- `128Mi` = 128 Mebibytes (MiB)
- `1Gi` = 1 Gibibyte (GiB)
- `256M` = 256 Megabytes (MB) - avoid, use Mi

---

### RBAC Verbs

- `get` - Read single resource
- `list` - List resources
- `watch` - Watch for changes
- `create` - Create resources
- `update` - Update resources
- `patch` - Patch resources
- `delete` - Delete resources
- `*` - All verbs

---

### Selector Operators

**Equality-based:**
- `=` or `==` - Equals
- `!=` - Not equals

**Set-based:**
- `in` - In list
- `notin` - Not in list
- `exists` - Key exists
- `!` (DoesNotExist) - Key doesn't exist

---

## Exam Tips

1. **Commands:** Always use array format
2. **Resources:** Know m (milli) and Mi/Gi units
3. **RBAC:** Practice creating roles and bindings quickly
4. **Quotas:** Understand hard limits vs defaults
5. **Labels:** Know both equality and set-based selectors
6. **Hooks:** postStart can fail pod startup

Good luck with your CKAD preparation!