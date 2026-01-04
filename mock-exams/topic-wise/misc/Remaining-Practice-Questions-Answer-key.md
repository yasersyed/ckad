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