# CKAD Jobs Practice Questions

## Application Design and Build (20%)

### Choose and Use the Right Workload Resource - Jobs

---

## Question 1: Basic Job
Create a Job named `data-import` that:
- Uses image `busybox`
- Runs command: `echo "Processing data" && sleep 30`
- Completes successfully once

---

## Question 2: Parallel Jobs
Create a Job named `batch-processor` that:
- Uses image `busybox`
- Runs command: `echo "Batch $HOSTNAME" && sleep 10`
- Runs 5 pods in parallel
- Completes 10 times total

---

## Question 3: Job with Retries
Create a Job named `flaky-task` that:
- Uses image `busybox`
- Runs command: `exit 1` (simulates failure)
- Retries up to 4 times (backoffLimit: 4)

---

## Question 4: Job with Deadline
Create a Job named `timed-task` that:
- Uses image `busybox`
- Runs command: `sleep 60`
- Has activeDeadlineSeconds: 30 (terminates after 30 seconds)

---

## Question 5: Job Cleanup
Create a Job named `temp-job` that:
- Uses image `busybox`
- Runs command: `echo "Done"`
- Automatically deletes after completion (ttlSecondsAfterFinished: 100)

---

## Question 6: Sequential Jobs
Create a Job named `sequential-processor` that:
- Uses image `busybox`
- Runs command: `echo "Task $HOSTNAME" && sleep 5`
- Runs 1 pod at a time (parallelism: 1)
- Completes 5 times total (completions: 5)

---

## Question 7: Job with RestartPolicy
Create a Job that:
- Uses image `busybox`
- Runs command that might fail
- Has restartPolicy: OnFailure (retries within same pod)

---

## Question 8: Troubleshoot Failed Job
A Job named `backup-db` has failed. Debug by:
- Checking Job status
- Viewing pod logs
- Checking why pods failed
- Fixing and recreating the Job

---

## Question 9: Job from CronJob
Create a Job manually from a CronJob template that:
- Uses the same spec as a CronJob
- Runs immediately (not on schedule)

---

## Question 10: Delete Completed Jobs
List and delete all completed Jobs in namespace `batch`:
- Find completed Jobs
- Delete them without deleting the pods first
- Clean up associated pods

---

## Question 11: Job with Resource Limits
Create a Job named `memory-task` that:
- Uses image `stress`
- Command: `stress --vm 1 --vm-bytes 256M --timeout 30s`
- Resource requests: memory 128Mi, cpu 100m
- Resource limits: memory 512Mi, cpu 500m

---

## Question 12: Monitor Job Progress
Create a Job and monitor it:
- Check Job status
- Watch pod creation
- View logs in real-time
- Determine when Job completes

---

## Key Concepts

### Job Specifications

**completions:** Number of successful completions required
- Default: 1
- Example: `completions: 5` (run until 5 pods succeed)

**parallelism:** Maximum number of pods running simultaneously
- Default: 1
- Example: `parallelism: 3` (run 3 pods at once)

**backoffLimit:** Number of retries before marking Job as failed
- Default: 6
- Example: `backoffLimit: 4` (retry 4 times)

**activeDeadlineSeconds:** Maximum time Job can run
- Example: `activeDeadlineSeconds: 300` (terminate after 5 minutes)

**ttlSecondsAfterFinished:** Cleanup Job after completion
- Example: `ttlSecondsAfterFinished: 100` (delete 100 seconds after finish)

### RestartPolicy Options

Jobs must use one of:
- **OnFailure:** Restart container on failure (same pod)
- **Never:** Don't restart container (create new pod)
- Cannot use `Always` (that's for Deployments/Pods)

### Job Patterns

**Single Job (run once):**
```yaml
completions: 1
parallelism: 1
```

**Parallel processing (fixed completions):**
```yaml
completions: 10
parallelism: 3
```

**Work queue (parallel until done):**
```yaml
parallelism: 5
# No completions specified
```

**Sequential processing:**
```yaml
completions: 5
parallelism: 1
```

---

## Essential kubectl Commands

### Creating Jobs
```bash
# Create Job from command
kubectl create job my-job --image=busybox -- echo "Hello"

# Generate YAML
kubectl create job my-job --image=busybox --dry-run=client -o yaml -- echo "Hello" > job.yaml

# Apply Job
kubectl apply -f job.yaml
```

### Monitoring Jobs
```bash
# Get Jobs
kubectl get jobs

# Get Job with status
kubectl get jobs -o wide

# Describe Job
kubectl describe job <job-name>

# Watch Job status
kubectl get jobs -w

# Get pods for a Job
kubectl get pods --selector=job-name=<job-name>
```

### Viewing Logs
```bash
# Get logs from Job pods
kubectl logs job/<job-name>

# Follow logs
kubectl logs job/<job-name> -f

# Logs from specific pod
kubectl logs <pod-name>
```

### Deleting Jobs
```bash
# Delete Job (keeps pods)
kubectl delete job <job-name>

# Delete Job and its pods
kubectl delete job <job-name> --cascade=foreground

# Delete all completed Jobs
kubectl delete jobs --field-selector status.successful=1

# Delete all failed Jobs
kubectl delete jobs --field-selector status.failed=1
```

### Job Status
```bash
# Check completion status
kubectl get jobs

# Output shows:
# COMPLETIONS: 3/5 (3 completed, 5 required)
# DURATION: 2m30s
# AGE: 5m

# Get detailed status
kubectl get job <job-name> -o yaml | grep -A 5 status:
```

---

## Common Job Patterns

### Basic Job Template
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: example-job
spec:
  template:
    spec:
      containers:
      - name: busybox
        image: busybox
        command: ["echo", "Hello World"]
      restartPolicy: Never
  backoffLimit: 4
```

### Parallel Job
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: parallel-job
spec:
  completions: 10
  parallelism: 3
  template:
    spec:
      containers:
      - name: worker
        image: busybox
        command: ["sh", "-c", "echo Processing $HOSTNAME && sleep 10"]
      restartPolicy: OnFailure
```

### Job with Deadline
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: timed-job
spec:
  activeDeadlineSeconds: 100
  template:
    spec:
      containers:
      - name: task
        image: busybox
        command: ["sleep", "120"]
      restartPolicy: Never
```

### Job with Auto-Cleanup
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: cleanup-job
spec:
  ttlSecondsAfterFinished: 100
  template:
    spec:
      containers:
      - name: task
        image: busybox
        command: ["echo", "Done"]
      restartPolicy: Never
```

### Job with Resource Limits
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: resource-job
spec:
  template:
    spec:
      containers:
      - name: task
        image: busybox
        command: ["sleep", "30"]
        resources:
          requests:
            memory: "64Mi"
            cpu: "100m"
          limits:
            memory: "128Mi"
            cpu: "200m"
      restartPolicy: Never
```

---

## Troubleshooting Jobs

### Check Job Status
```bash
# View Job details
kubectl describe job <job-name>

# Check events
kubectl get events --field-selector involvedObject.name=<job-name>

# View Job conditions
kubectl get job <job-name> -o jsonpath='{.status.conditions[*]}'
```

### Common Job Failures

**ImagePullBackOff:**
- Wrong image name
- Image doesn't exist
- No pull credentials

**CrashLoopBackOff:**
- Container exits with error
- Command fails immediately
- Check backoffLimit

**DeadlineExceeded:**
- Job runs longer than activeDeadlineSeconds
- Increase deadline or optimize task

**BackoffLimitExceeded:**
- Job failed more than backoffLimit times
- Fix the failing command
- Increase backoffLimit if needed

### Debug Commands
```bash
# Get pods from failed Job
kubectl get pods --selector=job-name=<job-name>

# Check pod status
kubectl describe pod <pod-name>

# View container logs
kubectl logs <pod-name>

# Check previous container logs (if crashed)
kubectl logs <pod-name> --previous

# Execute command in running pod
kubectl exec -it <pod-name> -- /bin/sh
```

---

## Job vs CronJob

| Feature | Job | CronJob |
|---------|-----|---------|
| Purpose | Run once or until completion | Run on schedule |
| Trigger | Manual or event | Time-based (cron) |
| Execution | Immediate | Scheduled |
| Use case | Batch processing, migrations | Backups, reports, cleanup |

---

## Exam Tips

1. **Use imperative commands** to generate YAML quickly
2. **Key fields to remember:**
   - `completions` (how many successes needed)
   - `parallelism` (how many at once)
   - `backoffLimit` (retry count)
   - `restartPolicy: Never` or `OnFailure` (not Always)
3. **Monitor with** `kubectl get jobs -w`
4. **Check logs with** `kubectl logs job/<job-name>`
5. **Delete with cascade** to clean up pods
6. **RestartPolicy must be Never or OnFailure** for Jobs
7. **TTL for cleanup** prevents clutter in cluster
8. **activeDeadlineSeconds** for time-bound tasks

---

## Practice Strategy

1. Create basic Job with single completion
2. Practice parallel Jobs with different parallelism values
3. Simulate failures and observe backoffLimit behavior
4. Use activeDeadlineSeconds for time-limited tasks
5. Practice troubleshooting failed Jobs
6. Monitor Job progress with kubectl commands
7. Clean up completed/failed Jobs

Good luck with your CKAD preparation!
