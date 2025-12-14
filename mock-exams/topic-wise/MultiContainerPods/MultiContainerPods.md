# Multi-Container Pod Design Patterns - Practice Questions

## Sidecar Pattern

**Question 1:** Create a Pod named `app-logger` with two containers:
- Main container: `nginx:alpine` named `web`
- Sidecar container: `busybox:latest` named `log-shipper` that tails the nginx access logs
- Use an emptyDir volume named `logs` mounted at `/var/log/nginx` in the web container and `/logs` in the sidecar
- The sidecar should run: `sh -c 'tail -f /logs/access.log'`

**Question 2:** Deploy a Pod with a main application container and a sidecar that performs log rotation. The sidecar should periodically compress old logs while the main container continues writing new ones.

**Question 3:** Create a monitoring sidecar that reads metrics from a shared volume where the main container writes performance data every 10 seconds.

## Init Container Pattern

**Question 4:** Create a Pod named `db-app` with:
- An init container named `init-db` using `busybox` that waits for a service named `mysql-service` to be available using `nslookup`
- Main container: `nginx:alpine` named `app`
- The init container should keep trying until the service resolves

**Question 5:** Build a Pod where an init container clones a Git repository into a shared volume before the main application container starts serving the files.

**Question 6:** Create a Pod with multiple init containers that run in sequence:
- First init container: checks if a ConfigMap exists
- Second init container: validates configuration files
- Main container: starts only after both validations pass

## Ambassador Pattern

**Question 7:** Create a Pod named `app-with-proxy` where:
- Main container: runs your application on port 8080
- Ambassador container: `haproxy` or `nginx` that proxies requests and handles SSL termination
- Use a shared emptyDir volume for configuration

**Question 8:** Deploy a Pod where an ambassador container manages database connection pooling for the main application container, abstracting the complexity of connecting to multiple database replicas.

## Adapter Pattern

**Question 9:** Create a Pod named `log-adapter` with:
- Main container: writes logs in custom JSON format to `/var/log/app.log`
- Adapter container: reads those logs and transforms them to a standard format (like syslog)
- Use emptyDir volume named `log-volume`

**Question 10:** Build a Pod where an adapter container converts application metrics from a proprietary format to Prometheus format, making them available on a standard `/metrics` endpoint.

## Mixed Patterns

**Question 11:** Create a complex Pod that uses both init and sidecar patterns:
- Init container: downloads application configuration from a remote source
- Main container: runs the application
- Sidecar container: monitors application health and writes status to a shared volume

**Question 12:** Design a Pod for a legacy application that:
- Uses an init container to migrate data format if needed
- Runs the main application
- Has a sidecar for shipping logs to Elasticsearch
- Has an adapter sidecar that translates the app's custom metrics format

## Troubleshooting Questions

**Question 13:** A Pod with init containers is stuck in `Init:0/2` status. How would you debug this? What kubectl commands would you use?

**Question 14:** Your sidecar container keeps restarting while the main container runs fine. The sidecar reads from a shared volume. What could be wrong and how would you fix it?

**Question 15:** You need to ensure a sidecar container starts after the main container has initialized. How would you achieve this since all regular containers start in parallel?

These questions test your understanding of when and how to use each pattern, volume sharing between containers, container lifecycle, and practical troubleshooting scenarios you'll face in the CKAD exam.
