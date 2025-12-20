# CKAD CronJob Practice Questions

## Question 1: Basic CronJob
Create a CronJob named `backup-daily` that:
- Runs every day at 2 AM (0 2 * * *)
- Uses image `busybox`
- Runs command: `echo "Backing up database"`

## Question 2: Frequent Schedule
Create a CronJob named `health-check` that:
- Runs every 5 minutes (*/5 * * * *)
- Uses image `busybox`
- Runs command: `wget -O- http://api-service:8080/health`

## Question 3: Job History Limits
Create a CronJob named `cleanup-logs` that:
- Runs every hour at minute 0 (0 * * * *)
- Uses image `busybox`
- Command: `rm -rf /tmp/old-logs`
- Keeps 3 successful job completions
- Keeps 1 failed job completion

## Question 4: Concurrency Policy
Create a CronJob named `report-generator` that:
- Runs every 15 minutes (*/15 * * * *)
- Uses image `busybox`
- Command: `echo "Generating report"`
- ConcurrencyPolicy: Forbid (prevent overlapping runs)

## Question 5: Replace Concurrency
Create a CronJob named `data-sync` that:
- Runs every 10 minutes (*/10 * * * *)
- Uses image `busybox`
- Command: `echo "Syncing data"`
- ConcurrencyPolicy: Replace (kill old job if still running)

## Question 6: Suspend CronJob
Create a CronJob named `maintenance-task`, then:
- Suspend it to prevent execution
- Resume it later
- Use imperative commands

## Question 7: Starting Deadline
Create a CronJob named `time-sensitive` that:
- Runs every minute (* * * * *)
- Uses image `busybox`
- startingDeadlineSeconds: 30 (skip if missed by 30+ seconds)

## Question 8: CronJob with Resource Limits
Create a CronJob named `batch-process` that:
- Runs every 6 hours (0 */6 * * *)
- Uses image `python:3.9`
- Command: `python process.py`
- Resource requests: memory 256Mi, cpu 200m
- Resource limits: memory 512Mi, cpu 500m

## Question 9: Multiple Jobs from CronJob
A CronJob named `hourly-backup` exists. Create:
- A manual Job from this CronJob to run immediately
- Verify it runs with the same configuration

## Question 10: CronJob with ConfigMap
Create a CronJob named `config-loader` that:
- Runs every hour (0 * * * *)
- Mounts ConfigMap `app-config` at `/config`
- Uses image `busybox`
- Command reads from `/config/settings.conf`

## Question 11: Update CronJob Schedule
A CronJob named `weekly-report` runs weekly. Update it to:
- Run daily at 3 AM instead
- Use imperative command or edit

## Question 12: Troubleshoot CronJob
A CronJob named `backup-db` isn't creating Jobs:
- Check if CronJob is suspended
- Verify the schedule syntax
- Check for startingDeadlineSeconds issues
- View CronJob events

## Question 13: Delete Old CronJob Jobs
A CronJob has created many Jobs. Clean up:
- List all Jobs created by the CronJob
- Delete completed Jobs
- Keep the CronJob running

## Question 14: CronJob with Secrets
Create a CronJob named `db-backup` that:
- Runs every day at midnight (0 0 * * *)
- Uses Secret `db-credentials` for database password
- Mounts Secret as environment variable

## Question 15: Parallel Job Execution
Create a CronJob named `batch-worker` that:
- Runs every hour
- Creates Jobs with parallelism: 3
- Creates Jobs with completions: 9

---

## CronJob Schedule Syntax Reference

```
* * * * *
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, Sunday=0 or 7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

## Common Schedules

```bash
*/5 * * * *      # Every 5 minutes
0 * * * *        # Every hour
0 0 * * *        # Every day at midnight
0 2 * * *        # Every day at 2 AM
0 0 * * 0        # Every Sunday at midnight
0 0 1 * *        # First day of month at midnight
*/15 * * * *     # Every 15 minutes
0 */6 * * *      # Every 6 hours
```

These match CKAD exam difficulty - focus on schedule syntax, concurrency policies, and job history limits!
