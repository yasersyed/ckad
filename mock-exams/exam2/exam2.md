# CKAD Mock Exam #2

**Exam Duration:** 2 hours  
**Format:** Practical, hands-on questions  
**Total Questions:** 16 scenarios  
**Passing Score:** ~66%

---

## Question 1: Multi-Container Pod with Shared Volume

Create a pod named `log-processor` with two containers:
- Container 1: `writer` using `busybox` image that writes the current timestamp to `/data/logs.txt` every 5 seconds
- Container 2: `reader` using `busybox` image that reads and displays the last 10 lines from `/data/logs.txt` every 10 seconds

Both containers should share an `emptyDir` volume mounted at `/data`. The pod should run indefinitely.

**Time Estimate:** 10 minutes

---

## Question 2: ConfigMap from Files

Create a ConfigMap named `app-properties` from the following files:
- Create a file `database.conf` with content: `host=db.example.com\nport=5432`
- Create a file `cache.conf` with content: `redis_host=cache.local\nredis_port=6379`

Then create a pod named `config-reader` that mounts this ConfigMap as a volume at `/etc/config` and lists the files in that directory.

**Time Estimate:** 8 minutes

---

## Question 3: Init Containers

Create a pod named `web-app` with:
- An init container named `setup` using `busybox` that creates a file `/work-dir/initialized.txt` with content "Setup Complete"
- A main container named `web` using `nginx` that mounts the same volume at `/usr/share/nginx/html`
- Use an `emptyDir` volume named `workdir`

Verify that the init container completes before the main container starts.

**Time Estimate:** 10 minutes

---

## Question 4: Rolling Update and Rollback

Create a deployment named `api-server` with:
- Image: `nginx:1.19`
- 4 replicas
- Labels: `app=api`, `version=v1`
- Strategy: RollingUpdate with maxSurge=1 and maxUnavailable=1

Perform the following:
1. Update the image to `nginx:1.20` and record the change
2. Check rollout status
3. Update to `nginx:1.21` with a wrong tag (e.g., `nginx:1.21-wrong`) and record
4. Observe the failure
5. Rollback to the previous working version
6. Verify all pods are running the correct version

**Time Estimate:** 12 minutes

---

## Question 5: Service Account and Secrets

Create the following:
- A ServiceAccount named `app-sa` in the `default` namespace
- A Secret named `api-credentials` with keys: `username=apiuser` and `token=abc123xyz`
- A pod named `secure-app` that:
  - Uses the ServiceAccount `app-sa`
  - Mounts the secret as environment variables
  - Uses image `nginx`

Verify the pod can access the secret values.

**Time Estimate:** 10 minutes

---

## Question 6: Horizontal Pod Autoscaler (HPA)

Create a deployment named `cpu-app` with:
- Image: `nginx`
- 2 replicas
- Resource requests: CPU 100m, Memory 128Mi
- Resource limits: CPU 200m, Memory 256Mi

Create a Horizontal Pod Autoscaler that:
- Targets the `cpu-app` deployment
- Scales between 2 and 10 replicas
- Targets 50% CPU utilization

Verify the HPA is created and monitoring the deployment.

**Time Estimate:** 8 minutes

---

## Question 7: Jobs with Parallelism

Create a Job named `batch-processor` that:
- Uses image `perl:5.34`
- Runs command: `perl -Mbignum=bpi -wle 'print bpi(2000)'` (calculates Pi)
- Requires 5 successful completions
- Runs 2 pods in parallel
- Has a backoff limit of 4
- Has an active deadline of 100 seconds

Monitor the job until completion and verify all 5 completions succeeded.

**Time Estimate:** 10 minutes

---

## Question 8: CronJob Scheduling

Create a CronJob named `backup-job` that:
- Runs every day at 3:00 AM
- Uses image `busybox`
- Executes command: `echo "Backup started at $(date)" && sleep 20 && echo "Backup completed"`
- Keeps history of last 3 successful jobs and 1 failed job
- Has a deadline of 60 seconds

Manually trigger the CronJob once to test it.

**Time Estimate:** 8 minutes

---

## Question 9: Pod Affinity and Anti-Affinity

Create two deployments:
1. Deployment `cache` with:
   - Image: `redis:6`
   - 2 replicas
   - Label: `app=cache`

2. Deployment `web-frontend` with:
   - Image: `nginx`
   - 3 replicas
   - Pod affinity: prefer to be scheduled on nodes where `cache` pods are running
   - Pod anti-affinity: avoid being scheduled on the same node as other `web-frontend` pods

Verify the scheduling behavior.

**Time Estimate:** 12 minutes

---

## Question 10: Readiness and Liveness Probes

Create a deployment named `health-app` with:
- Image: `nginx`
- 3 replicas
- Readiness probe: HTTP GET on port 80, path `/`, initial delay 5s, period 5s
- Liveness probe: HTTP GET on port 80, path `/`, initial delay 15s, period 10s, failure threshold 3
- Resources: requests (CPU 100m, memory 128Mi), limits (CPU 200m, memory 256Mi)

Simulate a failure scenario by exec into a pod and stopping nginx, then observe pod restart.

**Time Estimate:** 12 minutes

---

## Question 11: Network Policy - Egress Control

Create the following in a namespace called `restricted`:
- A pod named `app-pod` labeled `app=internal` using `busybox` that runs `sleep 3600`
- A pod named `database` labeled `app=database` using `nginx`
- A NetworkPolicy that:
  - Applies to pods with label `app=internal`
  - Allows egress traffic only to pods with label `app=database` on port 80
  - Allows DNS (port 53 UDP) to kube-system namespace

Test that `app-pod` can reach `database` but cannot reach external addresses.

**Time Estimate:** 12 minutes

---

## Question 12: Persistent Volume Claims

Create the following:
- A PersistentVolume named `local-pv` with:
  - Capacity: 2Gi
  - Access mode: ReadWriteOnce
  - Storage class: `manual`
  - Host path: `/mnt/data`
  
- A PersistentVolumeClaim named `app-pvc` requesting:
  - 1Gi storage
  - Access mode: ReadWriteOnce
  - Storage class: `manual`

- A pod named `pv-pod` that:
  - Uses the PVC
  - Mounts it at `/data`
  - Writes a file to `/data/test.txt`
  - Uses `busybox` image

Verify the PVC is bound and data persists.

**Time Estimate:** 10 minutes

---

## Question 13: Resource Quotas and Pod Priority

In a namespace called `dev-team`:
- Create a ResourceQuota that limits:
  - Total CPU requests: 4 cores
  - Total memory requests: 8Gi
  - Maximum pods: 20
  
- Create a PriorityClass named `high-priority` with value 1000
- Create a PriorityClass named `low-priority` with value 100

- Create two deployments:
  1. `critical-app` with 2 replicas, using `high-priority`, requesting 1 CPU and 1Gi memory each
  2. `background-app` with 3 replicas, using `low-priority`, requesting 500m CPU and 512Mi memory each

Verify the quota is enforced and priority classes are applied.

**Time Estimate:** 12 minutes

---

## Question 14: StatefulSet with Headless Service

Create a StatefulSet named `mongodb` with:
- 3 replicas
- Service name: `mongo`
- Image: `mongo:5.0`
- Volume claim template requesting 1Gi storage
- Environment variable: `MONGO_INITDB_ROOT_USERNAME=admin`
- Environment variable: `MONGO_INITDB_ROOT_PASSWORD=password`

Create a headless service for the StatefulSet. Verify each pod has a stable network identity and persistent storage.

**Time Estimate:** 12 minutes

---

## Question 15: Deployment Strategies - Blue/Green

Implement a blue/green deployment:
1. Create deployment `app-blue` with:
   - Image: `nginx:1.19`
   - 3 replicas
   - Label: `app=myapp`, `version=blue`

2. Create a service `app-service` that routes to `app=myapp` (version agnostic)

3. Create deployment `app-green` with:
   - Image: `nginx:1.20`
   - 3 replicas
   - Label: `app=myapp`, `version=green`

4. Switch traffic from blue to green by updating the service selector
5. Verify the switch and then clean up the old blue deployment

**Time Estimate:** 12 minutes

---

## Question 16: Troubleshooting Complex Scenario

You are given a broken deployment manifest (below). The deployment should run but currently fails. Identify and fix ALL issues:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: broken-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: web
        image: nginx:latest
        ports:
        - containerPort: 8080
        env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: db-config
              key: hostname
        volumeMounts:
        - name: data
          mountPath: /data
      volumes:
      - name: config
        configMap:
          name: app-config
```

Tasks:
1. Identify all issues in the manifest
2. Fix the issues
3. Create any missing resources (ConfigMaps)
4. Deploy successfully
5. Document what was wrong

**Time Estimate:** 15 minutes

---

## Exam Tips and Strategy

### Time Management
- **Quick wins first**: Questions 1, 2, 5, 6, 8 are faster - do these first
- **Complex scenarios**: Save 13, 15, 16 for when you have more time
- **Use imperative commands**: Generate YAML with `--dry-run=client -o yaml`

### Common Patterns to Remember
1. **Generate base YAML**: `kubectl run/create --dry-run=client -o yaml > file.yaml`
2. **Edit efficiently**: Use `kubectl edit` for quick changes
3. **Verify quickly**: `kubectl get pods -w` to watch status
4. **Check logs**: `kubectl logs` and `kubectl describe` are your friends
5. **Use documentation**: You can access Kubernetes docs during the exam

### Essential kubectl Commands
```bash
# Set context
kubectl config set-context --current --namespace=<namespace>

# Quick pod creation
kubectl run <name> --image=<image> --dry-run=client -o yaml

# Quick deployment
kubectl create deployment <name> --image=<image> --replicas=<n>

# Expose service
kubectl expose deployment <name> --port=<port> --target-port=<port>

# Scale
kubectl scale deployment <name> --replicas=<n>

# Update image
kubectl set image deployment/<name> <container>=<new-image>

# Rollout commands
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
```

### Resource Generation Shortcuts:
```bash
# ConfigMap from literal
kubectl create configmap <name> --from-literal=key=value

# Secret from literal
kubectl create secret generic <name> --from-literal=key=value

# Service account
kubectl create serviceaccount <name>

# Job
kubectl create job <name> --image=<image> -- <command>

# CronJob
kubectl create cronjob <name> --image=<image> --schedule="*/5 * * * *" -- <command>
```

---

## Scoring Guidelines

Each question is worth varying points based on complexity:
- Questions 1-3: 5 points each (15 total)
- Questions 4-8: 6 points each (30 total)
- Questions 9-13: 7 points each (35 total)
- Questions 14-15: 8 points each (16 total)
- Question 16: 10 points

**Total: 106 points**  
**Passing: 70 points (66%)**

Good luck! Remember to verify your work and manage your time wisely.
