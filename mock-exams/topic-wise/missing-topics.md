# Remaining Topics You Haven't Practiced

Based on your earlier request, here are the topics you're **still missing**:

## 1. Resource Management ⚠️ HIGH PRIORITY

### ❌ Not Covered Yet:
- **Resource requests and limits** (you've seen in examples, but no dedicated practice)
- **ResourceQuotas** (namespace-level limits)
- **LimitRanges** (default limits for pods/containers)

---

## 2. SecurityContext ⚠️ HIGH PRIORITY

### ❌ Completely Missing:
- **runAsUser** (run as specific user ID)
- **runAsNonRoot** (prevent root execution)
- **fsGroup** (file system group permissions)
- **capabilities** (add/drop Linux capabilities)
- **readOnlyRootFilesystem**
- **privileged** containers
- **securityContext** at pod vs container level

---

## 3. Environment Variables ⚠️ MEDIUM PRIORITY

### ❌ Not Covered Yet:
- **env** (simple key-value)
- **envFrom** (from ConfigMap/Secret)
- **valueFrom** (from field references, resource limits)
- **Environment variable substitution**

---

## 4. Commands and Arguments ⚠️ MEDIUM PRIORITY

### ❌ Not Covered Yet:
- **command vs args** (difference and usage)
- **Overriding ENTRYPOINT and CMD**
- **Using both command and args together**

---

## 5. Debugging and Troubleshooting ⚠️ HIGH PRIORITY

### ❌ Completely Missing:
- **kubectl logs** (viewing container logs)
- **kubectl exec** (executing commands in containers)
- **kubectl debug** (ephemeral debug containers)
- **kubectl get events** (viewing cluster events)
- **Troubleshooting crashed pods**
- **Troubleshooting ImagePullBackOff**
- **Troubleshooting CrashLoopBackOff**

---

## 6. Labels and Selectors ⚠️ MEDIUM PRIORITY

### ❌ Not Extensively Covered:
- **kubectl label** (adding/removing labels)
- **Label selectors** (equality-based, set-based)
- **Field selectors**
- **Filtering with -l flag**
- **Annotations** (vs labels)

---

## 7. Lifecycle Hooks ⚠️ LOWER PRIORITY

### ❌ Not Covered Yet:
- **postStart** hooks
- **preStop** hooks
- **Container lifecycle management**

---

## 8. Sidecar Containers ⚠️ LOWER PRIORITY

### ❌ Not Covered Yet:
- **Sidecar containers** (new in K8s 1.29+)
- **restartPolicy: Always for sidecars**

---

## 9. Advanced Topics ⚠️ LOWER PRIORITY

### ❌ Not Covered Yet:
- **Admission controllers** (basic awareness)
- **Pod Disruption Budgets**
- **Horizontal Pod Autoscaler**

---

## Priority Ranking for CKAD

### 🔴 Must Practice (Very Common):
1. **Debugging** (logs, exec, events)
2. **SecurityContext**
3. **Resource requests/limits**
4. **Environment variables**

### 🟡 Should Practice (Common):
5. **Commands and args**
6. **Labels and selectors**
7. **ResourceQuotas/LimitRanges**

### 🟢 Good to Know (Less Common):
8. **Lifecycle hooks**
9. **Sidecar containers**
10. **Admission controllers**

---

## Suggested Next Steps

Would you like me to create practice questions for:

1. **SecurityContext** (most important missing topic)
2. **Debugging** (kubectl logs, exec, troubleshooting)
3. **Environment Variables** (env, envFrom)
4. **Resource Management** (quotas, limits)

Which topic would you like to tackle next?
