# CKAD Debugging and Troubleshooting Practice Questions

## Application Observability and Maintenance (15%)

### Utilize container logs and Debugging in Kubernetes

---

## Question 1: View Container Logs
A Pod named `web-app` is running. View its logs for the last 20 lines.

---

## Question 2: Previous Container Logs
A Pod named `crasher` keeps restarting. View the logs from the previous container instance.

---

## Question 3: Multi-Container Logs
A Pod named `app-pod` has two containers: `nginx` and `sidecar`. View logs from the `sidecar` container only.

---

## Question 4: Follow Logs
Stream live logs from Pod `api-server` and save them to `/tmp/api-logs.txt`.

---

## Question 5: Logs with Timestamps
View logs from Pod `worker` with timestamps included.

---

## Question 6: Debug CrashLoopBackOff
A Pod named `broken-app` is in CrashLoopBackOff state. Debug and identify the issue.

---

## Question 7: Debug ImagePullBackOff
A Pod named `missing-image` is in ImagePullBackOff. Identify and fix the issue.

---

## Question 8: Execute Command in Pod
A Pod named `debug-pod` is running. Execute the command `df -h` inside it and save output to `/tmp/disk-usage.txt`.

---

## Question 9: Interactive Shell
Get an interactive shell in Pod `nginx-pod` and check if port 80 is listening.

---

## Question 10: Debug Init Container
A Pod named `init-app` is stuck in Init state. Debug the init container and identify the issue.

---

## Question 11: Check Pod Events
A Pod named `failing-pod` won't start. View the events to identify the problem.

---

## Question 12: Debug Pending Pod
A Pod named `pending-pod` is stuck in Pending state. Identify why it's not being scheduled.

---

## Question 13: Resource Issues
A Pod keeps getting OOMKilled. Identify which container is running out of memory and increase its limits.

---

## Question 14: Network Debugging
A Pod can't reach a Service. Debug the connectivity issue using a temporary pod.

---

## Question 15: ConfigMap/Secret Issues
A Pod fails to start with error about missing ConfigMap. Debug and fix the issue.

---

## Question 16: Volume Mount Issues
A Pod fails with "failed to mount volume" error. Debug and fix the volume configuration.

---

## Question 17: Liveness Probe Failures
A Pod keeps restarting due to liveness probe failures. Debug and fix the probe configuration.

---

## Question 18: Check Resource Usage
Check CPU and memory usage of all pods in namespace `production`.

---

## Question 19: Ephemeral Debug Container
A Pod is running but behaving incorrectly. Use kubectl debug to attach an ephemeral debug container.

---

## Question 20: Debug Node Issues
Pods on node `worker-1` are not starting. Debug the node and identify the issue.

---

## Key Concepts

### Container Logs

**kubectl logs** - View container output
```bash
kubectl logs <pod-name>
kubectl logs <pod-name> -c <container-name>
kubectl logs <pod-name> --previous
kubectl logs <pod-name> -f
kubectl logs <pod-name> --tail=50
kubectl logs <pod-name> --since=1h
kubectl logs <pod-name> --timestamps
```

### Execute Commands

**kubectl exec** - Run commands in containers
```bash
kubectl exec <pod-name> -- <command>
kubectl exec -it <pod-name> -- sh
kubectl exec <pod-name> -c <container-name> -- <command>
```

### Debug Containers

**kubectl debug** - Ephemeral debug containers
```bash
kubectl debug <pod-name> -it --image=busybox
kubectl debug node/<node-name> -it --image=busybox
```

### View Events

**kubectl describe** - Detailed information and events
```bash
kubectl describe pod <pod-name>
kubectl get events --sort-by='.lastTimestamp'
kubectl get events --field-selector involvedObject.name=<pod-name>
```

---

## Essential kubectl Commands

### Logs Commands

```bash
# Basic logs
kubectl logs <pod-name>

# Specific container
kubectl logs <pod-name> -c <container-name>

# Previous container (after crash)
kubectl logs <pod-name> --previous
kubectl logs <pod-name> -c <container-name> --previous

# Follow logs (stream)
kubectl logs <pod-name> -f
kubectl logs <pod-name> -f --tail=20

# Last N lines
kubectl logs <pod-name> --tail=50

# Time-based
kubectl logs <pod-name> --since=1h
kubectl logs <pod-name> --since=2024-01-01T10:00:00Z

# With timestamps
kubectl logs <pod-name> --timestamps

# All containers in pod
kubectl logs <pod-name> --all-containers=true

# Logs from all pods with label
kubectl logs -l app=web --all-containers=true
```

### Exec Commands

```bash
# Execute single command
kubectl exec <pod-name> -- ls /app

# Interactive shell
kubectl exec -it <pod-name> -- sh
kubectl exec -it <pod-name> -- bash
kubectl exec -it <pod-name> -- /bin/sh

# Specific container
kubectl exec -it <pod-name> -c <container-name> -- sh

# Execute with environment
kubectl exec <pod-name> -- env

# Check process
kubectl exec <pod-name> -- ps aux

# Network check
kubectl exec <pod-name> -- netstat -tulpn
kubectl exec <pod-name> -- curl localhost:8080
```

### Debug Commands

```bash
# Add ephemeral debug container
kubectl debug <pod-name> -it --image=busybox --target=<container-name>

# Debug with different image
kubectl debug <pod-name> -it --image=ubuntu -- bash

# Debug node
kubectl debug node/<node-name> -it --image=busybox

# Copy and debug
kubectl debug <pod-name> -it --copy-to=debug-pod --container=debug
```

### Describe and Events

```bash
# Describe pod (shows events)
kubectl describe pod <pod-name>

# Get all events
kubectl get events

# Sort by time
kubectl get events --sort-by='.lastTimestamp'

# Events for specific pod
kubectl get events --field-selector involvedObject.name=<pod-name>

# Events in namespace
kubectl get events -n <namespace>

# Watch events
kubectl get events -w
```

### Resource Usage

```bash
# CPU and memory usage
kubectl top pods
kubectl top pods -n <namespace>
kubectl top pod <pod-name>

# Node usage
kubectl top nodes

# Sort by CPU
kubectl top pods --sort-by=cpu

# Sort by memory
kubectl top pods --sort-by=memory

# All containers
kubectl top pods --containers
```

---

## Common Pod States and Issues

### CrashLoopBackOff

**Cause:** Container keeps crashing after starting

**Debug:**
```bash
# Check logs
kubectl logs <pod-name> --previous

# Check events
kubectl describe pod <pod-name>

# Common causes:
# - Application error
# - Missing dependency
# - Wrong command/args
# - Failed health checks
```

### ImagePullBackOff

**Cause:** Cannot pull container image

**Debug:**
```bash
kubectl describe pod <pod-name>

# Check:
# - Image name typo
# - Image doesn't exist
# - Missing imagePullSecrets
# - Registry auth issues
# - Network issues
```

**Fix:**
```bash
# Correct image name
kubectl set image pod/<pod-name> container=correct-image:tag

# Add imagePullSecret
kubectl create secret docker-registry regcred \
  --docker-server=<registry> \
  --docker-username=<user> \
  --docker-password=<pass>
```

### Pending

**Cause:** Pod cannot be scheduled

**Debug:**
```bash
kubectl describe pod <pod-name>

# Check events for:
# - Insufficient CPU/memory
# - No nodes available
# - Node selector/affinity mismatch
# - Taints/tolerations
# - PVC not bound
```

### Error / Failed

**Cause:** Container exited with error

**Debug:**
```bash
kubectl logs <pod-name>
kubectl describe pod <pod-name>

# Check exit code in pod status
kubectl get pod <pod-name> -o jsonpath='{.status.containerStatuses[0].lastState.terminated.exitCode}'
```

### OOMKilled

**Cause:** Out of memory

**Debug:**
```bash
kubectl describe pod <pod-name>
# Look for: Reason: OOMKilled

# Check memory limits
kubectl get pod <pod-name> -o yaml | grep -A 5 resources

# Increase memory limit
kubectl set resources deployment/<name> -c=<container> --limits=memory=512Mi
```

### Init:Error / Init:CrashLoopBackOff

**Cause:** Init container failing

**Debug:**
```bash
# Check init container logs
kubectl logs <pod-name> -c <init-container-name>

# Describe pod
kubectl describe pod <pod-name>

# List init containers
kubectl get pod <pod-name> -o jsonpath='{.spec.initContainers[*].name}'
```

---

## Troubleshooting Workflows

### Pod Won't Start

```bash
# 1. Check status
kubectl get pod <pod-name>

# 2. Describe for events
kubectl describe pod <pod-name>

# 3. Check logs
kubectl logs <pod-name>

# 4. Check previous logs if restarting
kubectl logs <pod-name> --previous

# 5. Check events
kubectl get events --field-selector involvedObject.name=<pod-name>
```

### Application Not Working

```bash
# 1. Check pod running
kubectl get pod <pod-name>

# 2. Check logs
kubectl logs <pod-name> -f

# 3. Exec into pod
kubectl exec -it <pod-name> -- sh

# 4. Test internally
kubectl exec <pod-name> -- curl localhost:8080

# 5. Check service
kubectl get svc
kubectl get endpoints <service-name>
```

### Network Issues

```bash
# 1. Test DNS
kubectl run test --rm -it --image=busybox -- nslookup <service-name>

# 2. Test connectivity
kubectl run test --rm -it --image=busybox -- wget -O- http://<service-name>

# 3. Check NetworkPolicies
kubectl get networkpolicies

# 4. Check service endpoints
kubectl get endpoints <service-name>

# 5. Port forward for testing
kubectl port-forward pod/<pod-name> 8080:80
```

### Volume Issues

```bash
# 1. Check PVC status
kubectl get pvc

# 2. Check PV
kubectl get pv

# 3. Describe PVC
kubectl describe pvc <pvc-name>

# 4. Check pod events
kubectl describe pod <pod-name>

# 5. Check mount inside pod
kubectl exec <pod-name> -- ls -la /mount/path
```

---

## Debug Examples

### Example 1: CrashLoopBackOff

```bash
# Pod keeps restarting
kubectl get pod broken-app
# STATUS: CrashLoopBackOff

# Check logs from previous container
kubectl logs broken-app --previous
# Error: cannot connect to database at localhost:5432

# Fix: Update database host
kubectl set env deployment/broken-app DB_HOST=database.default.svc.cluster.local
```

### Example 2: ImagePullBackOff

```bash
# Pod can't pull image
kubectl describe pod missing-image
# Events: Failed to pull image "ngnix:latest": image not found

# Fix: Correct image name
kubectl set image pod/missing-image container=nginx:latest
```

### Example 3: Pending Pod

```bash
# Pod stuck in Pending
kubectl describe pod pending-pod
# Events: 0/3 nodes available: insufficient memory

# Fix: Reduce memory request or add nodes
kubectl set resources deployment/app -c=container --requests=memory=256Mi
```

### Example 4: Service Not Working

```bash
# Service not routing traffic
kubectl get endpoints my-service
# ENDPOINTS: <none>

# Check pod labels
kubectl get pods --show-labels

# Fix selector mismatch
kubectl edit service my-service
# Change selector to match pod labels
```

### Example 5: Init Container Failing

```bash
# Pod stuck in Init
kubectl get pod init-app
# STATUS: Init:Error

# Check init container logs
kubectl logs init-app -c init-db
# Error: connection refused

# Fix init container
kubectl edit pod init-app
# Update init container command/image
```

---

## kubectl debug Examples

### Debug Running Pod

```bash
# Add debug container to running pod
kubectl debug pod/my-pod -it --image=busybox

# Inside debug container:
ps aux
ls /proc
netstat -tulpn
```

### Debug with Copy

```bash
# Create copy of pod for debugging
kubectl debug my-pod -it --copy-to=debug-pod --container=debug --image=ubuntu -- bash

# Original pod unchanged
# Debug pod has extra debug container
```

### Debug Node

```bash
# Debug node directly
kubectl debug node/worker-1 -it --image=ubuntu

# Inside node:
chroot /host
systemctl status kubelet
journalctl -u kubelet
```

---

## Exam Tips

1. **Always check logs first:** `kubectl logs pod --previous`
2. **Use describe for events:** `kubectl describe pod`
3. **Quick exec:** `kubectl exec -it pod -- sh`
4. **Follow logs:** `kubectl logs pod -f`
5. **Check events:** `kubectl get events --sort-by='.lastTimestamp'`
6. **Multi-container:** Use `-c container-name`
7. **Save output:** Use `> file.txt` or `tee`
8. **Network debug:** `kubectl run test --rm -it --image=busybox`
9. **Resource usage:** `kubectl top pods`
10. **Previous logs:** Essential for CrashLoopBackOff

---

## Common Debugging Scenarios

### Scenario 1: Pod Not Starting

```bash
kubectl get pod app
# Pending or Error

kubectl describe pod app | tail -20
# Check Events section

kubectl logs app
# If it started at all
```

### Scenario 2: Application Errors

```bash
kubectl logs app -f
# Follow logs to see errors

kubectl exec -it app -- sh
# Get shell to investigate
```

### Scenario 3: Networking Issues

```bash
kubectl get svc
kubectl get endpoints svc-name
# Check service has endpoints

kubectl run test --rm -it --image=busybox -- wget -O- http://svc-name
# Test from inside cluster
```

### Scenario 4: Resource Problems

```bash
kubectl top pods
# Check CPU/memory usage

kubectl describe pod app | grep -A 5 Limits
# Check resource limits
```

---

## Practice Strategy

1. Create broken pods and fix them
2. Practice all log commands (--previous, -f, --tail)
3. Use exec for interactive debugging
4. Practice checking events
5. Debug network connectivity
6. Fix ImagePullBackOff errors
7. Resolve resource constraints
8. Debug init containers
9. Fix liveness/readiness probe issues
10. Use kubectl debug for running pods

Good luck with your CKAD preparation!
