# CKAD SecurityContext Practice Questions

## Application Environment, Configuration and Security (25%)

### Understand Application Security (SecurityContexts, Capabilities, etc.)

---

## Question 1: Run as Non-Root User
Create a Pod named `secure-pod` with image `nginx` that runs as user ID `1000`.

---

## Question 2: Run as Non-Root (Required)
Create a Pod named `no-root` with image `busybox` that must run as a non-root user. If the image tries to run as root, it should be rejected.

---

## Question 3: Read-Only Root Filesystem
Create a Pod named `readonly-pod` with image `nginx` that has a read-only root filesystem.

---

## Question 4: Add Linux Capability
Create a Pod named `netadmin-pod` with image `nginx` that has the `NET_ADMIN` capability.

---

## Question 5: Drop All Capabilities
Create a Pod named `no-caps` with image `nginx` that drops all Linux capabilities.

---

## Question 6: Privileged Container
Create a Pod named `privileged-pod` with a privileged container using image `nginx`.

---

## Question 7: FSGroup for Volumes
Create a Pod named `volume-pod` with:
- Image: `busybox`
- emptyDir volume mounted at `/data`
- FSGroup: `2000`

---

## Question 8: Combined Security Settings
Create a Pod with:
- runAsUser: `1000`
- runAsGroup: `3000`
- fsGroup: `2000`
- runAsNonRoot: `true`

---

## Question 9: Container-Level vs Pod-Level
Create a Pod with two containers where:
- Pod-level: runAsUser `1000`
- Container 1: uses pod-level settings
- Container 2: overrides with runAsUser `2000`

---

## Question 10: Drop Specific Capabilities
Create a Pod that drops the `CHOWN` and `SETUID` capabilities.

---

## Question 11: SELinux Options
Create a Pod with SELinux options:
- level: `s0:c123,c456`

---

## Question 12: seccompProfile
Create a Pod with seccomp profile set to `RuntimeDefault`.

---

## Question 13: AppArmor Profile
Create a Pod with AppArmor profile `runtime/default` (using annotations).

---

## Question 14: Deployment with SecurityContext
Create a Deployment with 3 replicas that runs as user `1000` and group `3000`.

---

## Question 15: Privileged Pod Troubleshooting
A Pod is failing with "Operation not permitted". Add appropriate capabilities or privileged mode to fix it.

---

## Question 16: File Permissions with FSGroup
Create a Pod that writes to a volume. Verify that files created have group ID `2000`.

---

## Question 17: AllowPrivilegeEscalation
Create a Pod that prevents privilege escalation.

---

## Question 18: Multiple Security Settings
Create a Pod with:
- readOnlyRootFilesystem: true
- allowPrivilegeEscalation: false
- runAsNonRoot: true
- capabilities: drop ALL

---

## Question 19: PodSecurityContext vs SecurityContext
Explain the difference and create a Pod demonstrating both.

---

## Question 20: Security Context Troubleshooting
A Pod keeps getting killed. Check if it's due to SecurityContext settings and fix it.

---

## Key Concepts

### SecurityContext Levels

**Pod-level SecurityContext:**
```yaml
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    runAsNonRoot: true
```

**Container-level SecurityContext:**
```yaml
spec:
  containers:
  - name: myapp
    securityContext:
      runAsUser: 1000
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      capabilities:
        add: ["NET_ADMIN"]
        drop: ["ALL"]
```

**Precedence:** Container-level overrides Pod-level

### Common Fields

**User and Group:**
- `runAsUser`: User ID to run container
- `runAsGroup`: Group ID to run container
- `runAsNonRoot`: Require non-root user
- `fsGroup`: Group ID for volume ownership

**Filesystem:**
- `readOnlyRootFilesystem`: Make root filesystem read-only
- `fsGroup`: Set group ownership for volumes

**Privileges:**
- `privileged`: Run container in privileged mode
- `allowPrivilegeEscalation`: Allow/prevent privilege escalation
- `capabilities`: Add/drop Linux capabilities

**Security Profiles:**
- `seccompProfile`: Seccomp profile
- `seLinuxOptions`: SELinux context
- `appArmorProfile`: AppArmor profile (via annotations)

---

## Essential kubectl Commands

### View SecurityContext

```bash
# View pod security context
kubectl get pod <pod-name> -o yaml | grep -A 20 securityContext

# View specific fields
kubectl get pod <pod-name> -o jsonpath='{.spec.securityContext}'

# Check container security context
kubectl get pod <pod-name> -o jsonpath='{.spec.containers[0].securityContext}'
```

### Check User/Group in Running Pod

```bash
# Check what user container is running as
kubectl exec <pod-name> -- id

# Output: uid=1000 gid=3000 groups=2000

# Check file ownership
kubectl exec <pod-name> -- ls -la /data
```

### Troubleshoot SecurityContext Issues

```bash
# Check pod events for security errors
kubectl describe pod <pod-name>

# Look for errors like:
# Error: container has runAsNonRoot and image will run as root

# Check logs
kubectl logs <pod-name>
```

---

## YAML Examples

### Basic runAsUser

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-demo
spec:
  securityContext:
    runAsUser: 1000
  containers:
  - name: sec-demo
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
```

### runAsNonRoot

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: non-root-pod
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
  containers:
  - name: app
    image: nginx
```

### Read-Only Root Filesystem

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: readonly-pod
spec:
  containers:
  - name: nginx
    image: nginx
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: cache
      mountPath: /var/cache/nginx
    - name: run
      mountPath: /var/run
  volumes:
  - name: cache
    emptyDir: {}
  - name: run
    emptyDir: {}
```

### Add Capabilities

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: caps-pod
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
```

### Drop Capabilities

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: drop-caps-pod
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      capabilities:
        drop: ["ALL"]
```

### Drop All, Add Specific

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: minimal-caps-pod
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      capabilities:
        drop: ["ALL"]
        add: ["NET_BIND_SERVICE"]
```

### Privileged Container

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: privileged-pod
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      privileged: true
```

### FSGroup for Volumes

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: fsgroup-pod
spec:
  securityContext:
    fsGroup: 2000
  containers:
  - name: app
    image: busybox
    command: ["sh", "-c", "sleep 3600"]
    volumeMounts:
    - name: data
      mountPath: /data
  volumes:
  - name: data
    emptyDir: {}
```

### Complete Security Context

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    runAsNonRoot: true
  containers:
  - name: app
    image: nginx
    securityContext:
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
        add: ["NET_BIND_SERVICE"]
    volumeMounts:
    - name: cache
      mountPath: /var/cache/nginx
    - name: run
      mountPath: /var/run
  volumes:
  - name: cache
    emptyDir: {}
  - name: run
    emptyDir: {}
```

### Container Override Pod-Level

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: override-pod
spec:
  securityContext:
    runAsUser: 1000    # Pod-level
  containers:
  - name: container1
    image: busybox
    command: ["sleep", "3600"]
    # Uses pod-level runAsUser: 1000
  - name: container2
    image: busybox
    command: ["sleep", "3600"]
    securityContext:
      runAsUser: 2000  # Overrides pod-level
```

### Prevent Privilege Escalation

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: no-escalation
spec:
  containers:
  - name: app
    image: nginx
    securityContext:
      allowPrivilegeEscalation: false
```

### seccompProfile

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: seccomp-pod
spec:
  securityContext:
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    image: nginx
```

### SELinux Options

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: selinux-pod
spec:
  securityContext:
    seLinuxOptions:
      level: "s0:c123,c456"
  containers:
  - name: app
    image: nginx
```

### AppArmor (via annotations)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: apparmor-pod
  annotations:
    container.apparmor.security.beta.kubernetes.io/app: runtime/default
spec:
  containers:
  - name: app
    image: nginx
```

### Deployment with SecurityContext

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secure-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: secure
  template:
    metadata:
      labels:
        app: secure
    spec:
      securityContext:
        runAsUser: 1000
        runAsGroup: 3000
        fsGroup: 2000
        runAsNonRoot: true
      containers:
      - name: nginx
        image: nginx
        securityContext:
          readOnlyRootFilesystem: true
          allowPrivilegeEscalation: false
          capabilities:
            drop: ["ALL"]
        volumeMounts:
        - name: cache
          mountPath: /var/cache/nginx
        - name: run
          mountPath: /var/run
      volumes:
      - name: cache
        emptyDir: {}
      - name: run
        emptyDir: {}
```

### Maximum Security (Hardened)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hardened-pod
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    runAsNonRoot: true
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    image: nginx
    securityContext:
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
      runAsNonRoot: true
    volumeMounts:
    - name: cache
      mountPath: /var/cache/nginx
    - name: run
      mountPath: /var/run
  volumes:
  - name: cache
    emptyDir: {}
  - name: run
    emptyDir: {}
```

---

## Common Linux Capabilities

| Capability | Purpose |
|------------|---------|
| `NET_ADMIN` | Network administration |
| `NET_BIND_SERVICE` | Bind to ports < 1024 |
| `SYS_TIME` | Set system clock |
| `SYS_ADMIN` | System administration |
| `CHOWN` | Change file ownership |
| `SETUID` | Set user ID |
| `SETGID` | Set group ID |
| `DAC_OVERRIDE` | Bypass file permission checks |
| `KILL` | Send signals to processes |
| `ALL` | All capabilities |

---

## Troubleshooting

### Pod Won't Start - RunAsNonRoot Error

**Error:**
```
Error: container has runAsNonRoot and image will run as root
```

**Fix:**
```yaml
spec:
  securityContext:
    runAsUser: 1000     # Set non-root user
    runAsNonRoot: true
```

### Permission Denied - Read-Only Filesystem

**Error:**
```
cannot create directory: Read-only file system
```

**Fix:** Mount writable volumes for directories that need writes
```yaml
securityContext:
  readOnlyRootFilesystem: true
volumeMounts:
- name: tmp
  mountPath: /tmp
- name: cache
  mountPath: /var/cache
volumes:
- name: tmp
  emptyDir: {}
- name: cache
  emptyDir: {}
```

### Operation Not Permitted - Missing Capability

**Error:**
```
operation not permitted
```

**Fix:** Add required capability
```yaml
securityContext:
  capabilities:
    add: ["NET_ADMIN"]
```

### File Ownership Issues

**Problem:** Files created in volume have wrong ownership

**Fix:** Set fsGroup
```yaml
spec:
  securityContext:
    fsGroup: 2000
```

### Verify Settings

```bash
# Check user/group in pod
kubectl exec <pod> -- id
# uid=1000 gid=3000 groups=2000

# Check file permissions
kubectl exec <pod> -- ls -la /data
# drwxrwsr-x 2 root 2000 ...

# Check capabilities
kubectl exec <pod> -- capsh --print
```

---

## Security Best Practices

1. **Always run as non-root**
   ```yaml
   runAsNonRoot: true
   runAsUser: 1000
   ```

2. **Drop all capabilities by default**
   ```yaml
   capabilities:
     drop: ["ALL"]
   ```

3. **Use read-only root filesystem**
   ```yaml
   readOnlyRootFilesystem: true
   ```

4. **Prevent privilege escalation**
   ```yaml
   allowPrivilegeEscalation: false
   ```

5. **Add only required capabilities**
   ```yaml
   capabilities:
     drop: ["ALL"]
     add: ["NET_BIND_SERVICE"]
   ```

6. **Set fsGroup for shared volumes**
   ```yaml
   fsGroup: 2000
   ```

7. **Use seccomp profile**
   ```yaml
   seccompProfile:
     type: RuntimeDefault
   ```

8. **Avoid privileged containers**
   ```yaml
   privileged: false  # Default
   ```

---

## PodSecurityContext vs SecurityContext

**PodSecurityContext (spec.securityContext):**
- Applies to all containers in pod
- Fields: `runAsUser`, `runAsGroup`, `fsGroup`, `runAsNonRoot`, `seccompProfile`, `seLinuxOptions`

**Container SecurityContext (spec.containers[].securityContext):**
- Applies to specific container
- Can override pod-level settings
- Additional fields: `readOnlyRootFilesystem`, `allowPrivilegeEscalation`, `capabilities`, `privileged`

**Example:**
```yaml
spec:
  securityContext:           # Pod-level
    runAsUser: 1000
    fsGroup: 2000
  containers:
  - name: app
    securityContext:         # Container-level
      runAsUser: 2000        # Overrides pod-level
      readOnlyRootFilesystem: true
```

---

## Common Mistakes

### ❌ Mistake 1: Wrong Level

```yaml
# Wrong - capabilities at pod level
spec:
  securityContext:
    capabilities:        # ❌ Not valid at pod level
      drop: ["ALL"]
```

```yaml
# Correct - capabilities at container level
spec:
  containers:
  - name: app
    securityContext:
      capabilities:      # ✅ Valid at container level
        drop: ["ALL"]
```

### ❌ Mistake 2: runAsNonRoot Without runAsUser

```yaml
# Wrong - will fail if image defaults to root
spec:
  securityContext:
    runAsNonRoot: true   # ❌ No user specified
```

```yaml
# Correct
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000      # ✅ Specify non-root user
```

### ❌ Mistake 3: Read-Only Without Writable Volumes

```yaml
# Wrong - nginx needs writable /var/cache and /var/run
spec:
  containers:
  - name: nginx
    securityContext:
      readOnlyRootFilesystem: true  # ❌ nginx will fail
```

```yaml
# Correct - provide writable volumes
spec:
  containers:
  - name: nginx
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: cache
      mountPath: /var/cache/nginx
    - name: run
      mountPath: /var/run
  volumes:
  - name: cache
    emptyDir: {}
  - name: run
    emptyDir: {}
```

---

## Exam Tips

1. **Know the two levels:** Pod-level vs Container-level
2. **Common pod-level fields:** `runAsUser`, `runAsGroup`, `fsGroup`, `runAsNonRoot`
3. **Container-only fields:** `capabilities`, `readOnlyRootFilesystem`, `privileged`
4. **Drop ALL capabilities** is best practice
5. **Always set runAsUser** with `runAsNonRoot: true`
6. **Read-only filesystem** requires writable volumes for cache/temp
7. **Container settings override** pod settings
8. **fsGroup** sets group ownership for volumes
9. **Quick check:** `kubectl exec pod -- id` shows user/group
10. **Debugging:** `kubectl describe pod` shows security errors

---

## Practice Strategy

1. Create pods with different user IDs
2. Practice read-only filesystem with nginx (needs volumes)
3. Add and drop capabilities
4. Combine pod-level and container-level settings
5. Test runAsNonRoot with different images
6. Verify file ownership with fsGroup
7. Break and fix security context errors
8. Practice with Deployments (template spec)

Good luck with your CKAD preparation!
