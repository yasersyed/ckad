# CKAD Persistent Volumes and PVCs Practice Questions

## Application Design and Build (20%)

### Utilize Persistent and Ephemeral Volumes

---

## Question 1: Basic PVC
Create a PersistentVolumeClaim named `app-pvc` that requests 1Gi of storage with ReadWriteOnce access mode.

---

## Question 2: Pod with PVC
Create a Pod named `data-pod` using image `nginx` that mounts PVC `app-pvc` at `/data`.

---

## Question 3: PVC with Storage Class
Create a PVC named `fast-storage` requesting 2Gi from StorageClass `fast-ssd` with ReadWriteOnce access.

---

## Question 4: Multiple PVCs in Pod
Create a Pod named `multi-storage` with:
- PVC `config-pvc` mounted at `/config`
- PVC `data-pvc` mounted at `/data`
- Image: `busybox` running `sleep 3600`

---

## Question 5: Deployment with PVC
Create a Deployment named `web-app` with 3 replicas using PVC `shared-storage` mounted at `/app/data`. Use image `nginx`.

---

## Question 6: PVC Access Modes
Create a PVC named `multi-access` requesting 5Gi with ReadWriteMany access mode for shared storage across multiple pods.

---

## Question 7: emptyDir Volume
Create a Pod with two containers sharing an emptyDir volume:
- Container 1: `writer` writes to `/data`
- Container 2: `reader` reads from `/data`
- Volume mounted at `/data` in both

---

## Question 8: hostPath Volume
Create a Pod named `log-reader` that mounts the host's `/var/log` directory to `/host-logs` inside the container. Use image `busybox` with command `sleep 3600`.

---

## Question 9: ConfigMap as Volume
Create a ConfigMap named `app-config` with key `config.json`. Create a Pod that mounts this ConfigMap as a volume at `/etc/config`.

---

## Question 10: Secret as Volume
Create a Secret named `db-creds` with username and password. Create a Pod that mounts this Secret at `/etc/secrets`.

---

## Question 11: Troubleshoot PVC
A Pod named `app-pod` is stuck in Pending state with PVC `storage-claim`. Debug why the PVC isn't binding.

---

## Question 12: PVC with Specific Size
Create a PVC requesting exactly 500Mi of storage. Create a Pod using this PVC.

---

## Question 13: ReadWriteOnce vs ReadWriteMany
Create two PVCs:
- `rwo-pvc`: ReadWriteOnce, 1Gi
- `rwx-pvc`: ReadWriteMany, 1Gi
Create Pods to test each access mode.

---

## Question 14: Update Pod Volume Mount
A Pod exists with a volume mounted at `/old-path`. Update it to mount at `/new-path` instead.

---

## Question 15: Volume with subPath
Create a Pod that mounts a PVC at `/data` but uses subPath to mount only the `logs` subdirectory.

---

## Question 16: PVC Status
Check the status of PVC `my-claim`. Determine if it's Bound, Pending, or Lost.

---

## Question 17: Delete PVC
Delete PVC `old-storage` and verify the associated PersistentVolume's reclaim policy behavior.

---

## Question 18: Ephemeral Volume Types
Create a Pod using:
- emptyDir with memory medium (tmpfs)
- Size limit of 256Mi
- Image: `busybox` running `sleep 3600`

---

## Question 19: PVC in StatefulSet
Create a StatefulSet with volumeClaimTemplates that automatically creates PVCs for each replica.

---

## Question 20: Shared Volume Between Containers
Create a Pod with init container that writes data to a volume, and main container that reads it.

---

## Key Concepts

### PersistentVolume (PV)
- Cluster-level storage resource
- Provisioned by admin or dynamically
- Lifecycle independent of pods
- Has capacity, access modes, reclaim policy

### PersistentVolumeClaim (PVC)
- Request for storage by user/pod
- Binds to available PV
- Pods use PVCs to access storage
- Specifies size and access mode

### Access Modes
- **ReadWriteOnce (RWO)**: Single node read-write
- **ReadOnlyMany (ROX)**: Multiple nodes read-only
- **ReadWriteMany (RWX)**: Multiple nodes read-write
- **ReadWriteOncePod (RWOP)**: Single pod read-write (1.22+)

### Volume Types

**Ephemeral Volumes:**
- emptyDir: Empty directory, pod lifetime
- configMap: Configuration data
- secret: Sensitive data
- downwardAPI: Pod/container metadata

**Persistent Volumes:**
- PersistentVolumeClaim
- HostPath (single node only)
- NFS, iSCSI, Cloud provider volumes

### Reclaim Policies
- **Retain**: Keep data after PVC deleted
- **Delete**: Delete PV and data after PVC deleted
- **Recycle**: Basic scrub (deprecated)

---

## Essential kubectl Commands

### PVC Operations
```bash
# Create PVC
kubectl create -f pvc.yaml

# List PVCs
kubectl get pvc

# Describe PVC
kubectl describe pvc <pvc-name>

# Check PVC status
kubectl get pvc <pvc-name> -o jsonpath='{.status.phase}'

# Delete PVC
kubectl delete pvc <pvc-name>

# Get PVC capacity
kubectl get pvc <pvc-name> -o jsonpath='{.spec.resources.requests.storage}'
```

### PV Operations
```bash
# List PVs
kubectl get pv

# Describe PV
kubectl describe pv <pv-name>

# Check PV status
kubectl get pv <pv-name> -o jsonpath='{.status.phase}'

# Check which PVC bound to PV
kubectl get pv <pv-name> -o jsonpath='{.spec.claimRef.name}'
```

### Pod Volume Operations
```bash
# Create pod with volume
kubectl apply -f pod-with-volume.yaml

# Check pod volumes
kubectl describe pod <pod-name> | grep -A 10 Volumes

# Check volume mounts
kubectl describe pod <pod-name> | grep -A 10 Mounts

# Execute into pod and check mount
kubectl exec -it <pod-name> -- df -h
kubectl exec -it <pod-name> -- ls -la /data
```

---

## YAML Examples

### Basic PVC
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

### PVC with StorageClass
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: fast-storage
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: fast-ssd
  resources:
    requests:
      storage: 2Gi
```

### PVC with ReadWriteMany
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: shared-pvc
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 5Gi
```

### Pod with PVC
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: data-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: storage
      mountPath: /data
  volumes:
  - name: storage
    persistentVolumeClaim:
      claimName: app-pvc
```

### Pod with Multiple PVCs
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-storage
spec:
  containers:
  - name: app
    image: busybox
    command: ["sleep", "3600"]
    volumeMounts:
    - name: config-vol
      mountPath: /config
    - name: data-vol
      mountPath: /data
  volumes:
  - name: config-vol
    persistentVolumeClaim:
      claimName: config-pvc
  - name: data-vol
    persistentVolumeClaim:
      claimName: data-pvc
```

### Deployment with PVC
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
        volumeMounts:
        - name: data
          mountPath: /app/data
      volumes:
      - name: data
        persistentVolumeClaim:
          claimName: shared-storage
```

### emptyDir Volume
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: shared-data
spec:
  containers:
  - name: writer
    image: busybox
    command: ["sh", "-c", "echo hello > /data/file.txt && sleep 3600"]
    volumeMounts:
    - name: shared
      mountPath: /data
  - name: reader
    image: busybox
    command: ["sh", "-c", "cat /data/file.txt && sleep 3600"]
    volumeMounts:
    - name: shared
      mountPath: /data
  volumes:
  - name: shared
    emptyDir: {}
```

### emptyDir with Memory Medium
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: memory-pod
spec:
  containers:
  - name: app
    image: busybox
    command: ["sleep", "3600"]
    volumeMounts:
    - name: cache
      mountPath: /cache
  volumes:
  - name: cache
    emptyDir:
      medium: Memory
      sizeLimit: 256Mi
```

### hostPath Volume
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: log-reader
spec:
  containers:
  - name: reader
    image: busybox
    command: ["sleep", "3600"]
    volumeMounts:
    - name: host-logs
      mountPath: /host-logs
      readOnly: true
  volumes:
  - name: host-logs
    hostPath:
      path: /var/log
      type: Directory
```

### ConfigMap as Volume
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  config.json: |
    {
      "env": "production",
      "debug": false
    }
---
apiVersion: v1
kind: Pod
metadata:
  name: config-pod
spec:
  containers:
  - name: app
    image: busybox
    command: ["sleep", "3600"]
    volumeMounts:
    - name: config
      mountPath: /etc/config
  volumes:
  - name: config
    configMap:
      name: app-config
```

### Secret as Volume
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-creds
type: Opaque
data:
  username: YWRtaW4=  # admin
  password: cGFzc3dvcmQxMjM=  # password123
---
apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
spec:
  containers:
  - name: app
    image: busybox
    command: ["sleep", "3600"]
    volumeMounts:
    - name: secrets
      mountPath: /etc/secrets
      readOnly: true
  volumes:
  - name: secrets
    secret:
      secretName: db-creds
```

### Volume with subPath
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: subpath-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: data
      mountPath: /app/logs
      subPath: logs  # Only mount logs subdirectory
  volumes:
  - name: data
    persistentVolumeClaim:
      claimName: app-pvc
```

### StatefulSet with VolumeClaimTemplates
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: nginx
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
        image: nginx
        volumeMounts:
        - name: data
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

### Init Container with Shared Volume
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-pod
spec:
  initContainers:
  - name: init
    image: busybox
    command: ["sh", "-c", "echo 'Initialized' > /work/ready.txt"]
    volumeMounts:
    - name: workdir
      mountPath: /work
  containers:
  - name: main
    image: busybox
    command: ["sh", "-c", "cat /work/ready.txt && sleep 3600"]
    volumeMounts:
    - name: workdir
      mountPath: /work
  volumes:
  - name: workdir
    emptyDir: {}
```

---

## Troubleshooting

### PVC Stuck in Pending

**Common causes:**
```bash
# 1. No available PV
kubectl get pv
# No PV matches size/access mode

# 2. StorageClass doesn't exist
kubectl get storageclass
kubectl describe pvc <pvc-name>

# 3. Insufficient capacity
kubectl describe pvc <pvc-name>
# Check Events section

# 4. Access mode mismatch
kubectl get pv -o custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage,ACCESS:.spec.accessModes
```

**Debug steps:**
```bash
# Check PVC status
kubectl get pvc <pvc-name>

# Describe for events
kubectl describe pvc <pvc-name>

# Check available PVs
kubectl get pv

# Check StorageClass
kubectl get sc
```

### Pod Stuck in Pending Due to Volume

```bash
# Check pod events
kubectl describe pod <pod-name>

# Look for:
# - "PersistentVolumeClaim is not bound"
# - "no persistent volumes available"
# - "volume node affinity conflict"

# Check PVC status
kubectl get pvc
```

### Volume Not Mounting

```bash
# Check pod describe
kubectl describe pod <pod-name>

# Verify volume definition
kubectl get pod <pod-name> -o yaml | grep -A 10 volumes

# Check inside pod
kubectl exec -it <pod-name> -- df -h
kubectl exec -it <pod-name> -- mount | grep <mount-path>
```

### Permission Issues

```bash
# Check volume permissions
kubectl exec -it <pod-name> -- ls -la /data

# May need securityContext:
# runAsUser, fsGroup, etc.
```

---

## Important Notes

### PVC Binding
- PVC binds to smallest available PV that satisfies requirements
- Binding is 1:1 (one PVC to one PV)
- Once bound, PVC is exclusive until deleted

### Access Modes and Volume Types
| Volume Type | RWO | ROX | RWX |
|-------------|-----|-----|-----|
| emptyDir | ✅ | ✅ | ✅ |
| hostPath | ✅ | ✅ | ✅ |
| NFS | ✅ | ✅ | ✅ |
| EBS | ✅ | ❌ | ❌ |
| GCE PD | ✅ | ✅ | ❌ |

### StorageClass
- Dynamic provisioning
- Creates PV automatically when PVC created
- Default StorageClass used if not specified
- Check default: `kubectl get sc`

### Volume Lifecycle
1. **Provisioning**: PV created (static or dynamic)
2. **Binding**: PVC binds to PV
3. **Using**: Pod mounts PVC
4. **Releasing**: PVC deleted
5. **Reclaiming**: PV reclaimed per policy

---

## Exam Tips

1. **PVC requires**: accessModes, resources.requests.storage
2. **Pod volume mount**: name must match volume name
3. **emptyDir**: Pod-scoped, deleted when pod deleted
4. **hostPath**: Node-specific, use cautiously
5. **PVC in Deployment**: All replicas share same PVC (use StatefulSet for per-pod PVCs)
6. **Check PVC status**: `kubectl get pvc` shows Bound/Pending
7. **subPath**: Mount specific subdirectory of volume
8. **ReadWriteMany**: Not supported by all volume types
9. **Memory medium**: emptyDir uses RAM (fast but volatile)
10. **Init containers**: Share volumes with main containers

---

## Practice Strategy

1. Create PVCs with different sizes and access modes
2. Mount PVCs in Pods and Deployments
3. Practice emptyDir for shared data between containers
4. Use ConfigMaps and Secrets as volumes
5. Debug pending PVCs
6. Practice StatefulSet with volumeClaimTemplates
7. Test different volume types (hostPath, emptyDir)
8. Understand when to use each volume type

Good luck with your CKAD preparation!
