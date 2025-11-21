# CKAD Mock Exam

**Exam Duration:** 2 hours  
**Format:** Practical, hands-on questions  
**Total Questions:** 15 scenarios  
**Passing Score:** ~66%

---

## Question 1: Pod Creation and Troubleshooting

Create a pod named `web-server` in the `production` namespace that:
- Uses the `nginx:latest` image
- Exposes port 80
- Has CPU requests of 100m and limits of 500m
- Has memory requests of 128Mi and limits of 256Mi

Verify the pod is running and describe its resource allocation.

**Time Estimate:** 5 minutes

---

## Question 2: ConfigMaps and Secrets

Create a ConfigMap named `app-config` with the following key-value pairs:
- `DATABASE_HOST=db.example.com`
- `DATABASE_PORT=5432`
- `LOG_LEVEL=info`

Then create a Secret named `app-secrets` with:
- `database_user=admin`
- `database_password=securepass123`

Create a pod named `app-pod` that uses both ConfigMap and Secret as environment variables. Verify the environment variables are properly set.

**Time Estimate:** 8 minutes

---

## Question 3: Multi-Container Pod

Create a pod named `multi-app` with two containers:
- Container 1: `app` using `nginx:1.21` image, exposing port 80
- Container 2: `sidecar` using `busybox:latest` image, running the command `sleep 3600`

Both containers should share a volume of type `emptyDir` mounted at `/shared-data`. Use init containers to ensure the shared volume is properly initialized before the main containers start.

**Time Estimate:** 10 minutes

---

## Question 4: Deployment Management

Create a Deployment named `web-app` with:
- 3 replicas
- Image: `nginx:1.20`
- Labels: `app=web`, `tier=frontend`
- Resource limits: 200m CPU, 256Mi memory
- Resource requests: 100m CPU, 128Mi memory

Perform a rolling update to image version `nginx:1.21` and monitor the rollout status. Then rollback to the previous version.

**Time Estimate:** 10 minutes

---

## Question 5: Service Configuration

Create a Service named `backend-svc` that:
- Routes traffic to pods with label `app=backend`
- Uses ClusterIP service type
- Exposes port 8080, targeting port 8080 on pods
- Is created in the `default` namespace

Create a test pod in the `default` namespace and verify connectivity to the service using DNS name `backend-svc.default.svc.cluster.local`.

**Time Estimate:** 8 minutes

---

## Question 6: Namespace Management

Create three namespaces: `development`, `staging`, and `production`. Create a pod named `app` in each namespace using `nginx:latest`. Use kubectl to verify all pods across all three namespaces and then delete the `development` namespace along with all its resources.

**Time Estimate:** 7 minutes

---

## Question 7: PersistentVolume and PersistentVolumeClaim

Create a PersistentVolume named `local-pv` with:
- Capacity: 5Gi
- Access mode: ReadWriteOnce
- Storage class: `local-storage`
- Host path: `/mnt/data`

Create a PersistentVolumeClaim named `local-pvc` that requests 2Gi of storage with the same storage class. Create a pod that uses this PVC and mounts it at `/data`. Verify the claim is bound to the volume.

**Time Estimate:** 10 minutes

---

## Question 8: Probes and Pod Lifecycle

Create a pod named `health-check-pod` using `nginx:latest` that includes:
- A liveness probe using HTTP GET on port 80, path `/`, with initial delay of 30s and period of 10s
- A readiness probe using HTTP GET on port 80, path `/`, with initial delay of 5s and period of 5s
- Resource requests and limits as appropriate

Verify the probes are working by monitoring the pod status.

**Time Estimate:** 8 minutes

---

## Question 9: StatefulSet

Create a StatefulSet named `database` with:
- 3 replicas
- Service name: `database`
- Image: `postgres:13`
- Environment variables for database setup (POSTGRES_USER=admin, POSTGRES_PASSWORD=password)
- PVC template requesting 2Gi of storage

Verify the StatefulSet creates pods with stable names (database-0, database-1, database-2) and stable DNS names.

**Time Estimate:** 10 minutes

---

## Question 10: DaemonSet

Create a DaemonSet named `node-monitor` that:
- Uses image `busybox:latest`
- Runs the command: `watch -n 5 'date'`
- Has a nodeSelector for nodes labeled `monitoring=enabled`
- Includes appropriate tolerations for tainted nodes

Verify the DaemonSet is running on all matching nodes.

**Time Estimate:** 8 minutes

---

## Question 11: Jobs and CronJobs

Create a Job named `batch-process` that:
- Runs the image `busybox:latest`
- Executes the command: `sh -c 'echo "Processing data..." && sleep 10 && echo "Done"'`
- Has 3 completions and parallelism of 2
- Has a backoff limit of 3

Monitor the job completion and then create a CronJob that runs this same job every day at 2 AM.

**Time Estimate:** 10 minutes

---

## Question 12: Resource Quotas and Limits

Create a namespace called `limited-ns`. Define a ResourceQuota for this namespace that:
- Limits total CPU requests to 10 cores
- Limits total memory requests to 20Gi
- Limits the number of pods to 50

Create a LimitRange that:
- Sets default CPU limit to 500m
- Sets default memory limit to 512Mi
- Sets minimum CPU to 100m
- Sets minimum memory to 128Mi

Verify the quota and limits are applied when creating pods in this namespace.

**Time Estimate:** 10 minutes

---

## Question 13: Network Policies

Create a NetworkPolicy named `deny-ingress` that:
- Denies all incoming traffic to pods labeled `app=secure`
- Allows traffic only from pods labeled `app=trusted` on port 8080

Create two pods: one labeled `app=secure` and one labeled `app=trusted`. Verify the network policy enforcement.

**Time Estimate:** 10 minutes

---

## Question 14: Debugging and Logging

Create a pod named `crash-pod` that deliberately fails to start. Use kubectl commands to:
- Describe the pod and identify why it failed
- View logs (if available)
- Get events related to the pod
- Fix the pod manifest and ensure it runs successfully

Document the troubleshooting steps you took.

**Time Estimate:** 8 minutes

---

## Question 15: Application Deployment Scenario

Create a complete application deployment consisting of:
- A Deployment for `frontend` (nginx) with 2 replicas
- A Deployment for `backend` (using a simple Python/Go image or busybox simulation)
- Services to expose both frontend and backend
- ConfigMap for frontend configuration
- A pod that tests connectivity between frontend and backend services

Document the architecture and verify end-to-end connectivity.

**Time Estimate:** 15 minutes

---

## Tips for Success

1. **Practice with kubectl**: Learn to use imperative commands (`kubectl run`, `kubectl create`, etc.) and understand declarative manifests
2. **Time Management**: Allocate your time wisely. Spend more time on complex scenarios
3. **Use Shortcuts**: Learn `--dry-run=client -o yaml` to generate manifests quickly
4. **Know your API Objects**: Be familiar with Pods, Deployments, Services, StatefulSets, DaemonSets, Jobs, CronJobs, ConfigMaps, Secrets, PV, and PVC
5. **Practice Troubleshooting**: Know how to use `kubectl logs`, `kubectl describe`, `kubectl get events`
6. **Namespace Awareness**: Many scenarios require working across multiple namespaces
7. **Resource Management**: Understand requests, limits, and quality of service classes
8. **Label and Selectors**: Master using labels and selectors for pod identification and management

---

## Answer Guide Summary

For detailed answers and implementation strategies, ensure you can:
- Write correct YAML manifests using both `kubectl` imperative commands and declarative files
- Understand pod scheduling, lifecycle, and networking
- Troubleshoot common issues using kubectl debugging tools
- Manage application configuration and secrets securely
- Implement persistent storage and stateful applications
- Monitor and maintain application health through probes
- Apply resource constraints and quotas appropriately
