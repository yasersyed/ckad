# CKAD Services and Service Discovery Practice Questions

## Services and Networking (20%)

### Services, Service Discovery, and DNS

---

## Question 1: ClusterIP Service
Create a Service named `backend-svc` in namespace `default` that exposes pods
 with label `app=backend` on port 80, targeting container port 8080.

---

## Question 2: NodePort Service
Create a Service named `web-nodeport` that exposes Deployment `web-app` 
on NodePort 30080, service port 80, targeting pod port 8080.

---

## Question 3: Multiple Ports
Create a Service named `multi-port-svc` for pods labeled `app=api` that exposes:
- Port 80 (named "http") → target port 8080
- Port 443 (named "https") → target port 8443

---

## Question 4: Headless Service
Create a headless Service named `mysql-headless` for StatefulSet pods with label `app=mysql` 
on port 3306.

---

## Question 5: ExternalName Service
Create a Service named `external-db` that maps to external database `database.company.com`.

---

## Question 6: Service Endpoints
A Service named `api-svc` exists but returns no endpoints. Troubleshoot why pods aren't being selected.

---

## Question 7: DNS Resolution
Create a pod that uses `nslookup` to resolve Service `backend-svc` in namespace `production`. Capture the output.

---

## Question 8: Cross-Namespace Communication
Create two Deployments: `frontend` in namespace `web` and `backend` in namespace `api`. Create Services so frontend can access backend using DNS.

---

## Question 9: Service Without Selector
Create a Service named `legacy-app` on port 80 without a selector. Manually create an Endpoints object pointing to IP `10.0.0.5` on port 8080.

---

## Question 10: Update Service
A Service named `app-svc` exists with type ClusterIP. Update it to type NodePort with NodePort 30090.

---

## Question 11: Service Labels
Create a Service that selects pods with labels `app=web` AND `env=production`.

---

## Question 12: DNS FQDN
From a pod in namespace `default`, access Service `api-svc` in namespace `backend` using the fully qualified domain name.

---

## Question 13: Service Session Affinity
Create a Service named `sticky-svc` with session affinity set to ClientIP for pods labeled `app=session-app`.

---

## Question 14: LoadBalancer Service
Create a Service of type LoadBalancer named `public-web` exposing Deployment `nginx-app` on port 80.

---

## Question 15: Service Discovery Test
Create a Deployment and Service. Then create a test pod that curls the Service endpoint and logs the response.

---

## Question 16: Service Port Naming
Create a Service with named ports where the Service references the port by name instead of number.

---

## Question 17: Delete Service
Delete Service `old-svc` without deleting the pods it was selecting.

---

## Question 18: Service IP
Get the ClusterIP of Service `backend-svc` and store it in a file named `service-ip.txt`.

---

## Question 19: Service to StatefulSet
Create a Service that works with a StatefulSet, providing stable network identities for each pod.

---

## Question 20: Troubleshoot DNS
A pod cannot resolve service names. Debug and fix the DNS resolution issue.

---

## Key Concepts

### Service Types

**ClusterIP (default):**
- Internal cluster IP
- Only accessible within cluster
- Most common type

**NodePort:**
- Exposes service on each node's IP at a static port
- Accessible from outside cluster: `<NodeIP>:<NodePort>`
- Port range: 30000-32767

**LoadBalancer:**
- Cloud provider load balancer
- Exposes service externally
- Gets external IP address

**ExternalName:**
- Maps service to external DNS name
- No proxying, just DNS CNAME
- No selector or ports

**Headless (ClusterIP: None):**
- No cluster IP assigned
- DNS returns pod IPs directly
- Used with StatefulSets

### DNS Naming Convention

```
<service-name>.<namespace>.svc.cluster.local
```

**Examples:**
- Same namespace: `backend-svc`
- Different namespace: `backend-svc.production`
- FQDN: `backend-svc.production.svc.cluster.local`

### Service Discovery

Pods can discover services via:
1. **Environment variables** (injected at pod creation)
2. **DNS** (recommended - dynamic)

DNS automatically created for each service:
```
<service>.<namespace>.svc.cluster.local → Service ClusterIP
```

For headless services:
```
<pod-name>.<service>.<namespace>.svc.cluster.local → Pod IP
```

---

## Essential kubectl Commands

### Creating Services

```bash
# Expose deployment as ClusterIP
kubectl expose deployment <name> --port=80 --target-port=8080

# Create NodePort service
kubectl expose deployment <name> --type=NodePort --port=80 --target-port=8080

# Create LoadBalancer service
kubectl expose deployment <name> --type=LoadBalancer --port=80

# Create service from YAML
kubectl apply -f service.yaml

# Generate service YAML
kubectl expose deployment <name> --port=80 --dry-run=client -o yaml > svc.yaml
```

### Viewing Services

```bash
# List services
kubectl get svc
kubectl get services

# Describe service
kubectl describe svc <service-name>

# Get service details
kubectl get svc <service-name> -o wide
kubectl get svc <service-name> -o yaml

# Get service endpoints
kubectl get endpoints <service-name>
kubectl get ep <service-name>

# Get service ClusterIP
kubectl get svc <service-name> -o jsonpath='{.spec.clusterIP}'
```

### Testing Services

```bash
# DNS lookup from pod
kubectl run test --rm -it --image=busybox -- nslookup <service-name>

# Test service connectivity
kubectl run test --rm -it --image=busybox -- wget -O- <service-name>:<port>

# Curl service from pod
kubectl run curl --rm -it --image=curlimages/curl -- curl http://<service-name>

# Port forward to local machine
kubectl port-forward svc/<service-name> 8080:80
```

### Updating Services

```bash
# Edit service
kubectl edit svc <service-name>

# Patch service type
kubectl patch svc <service-name> -p '{"spec":{"type":"NodePort"}}'

# Set NodePort
kubectl patch svc <service-name> -p '{"spec":{"ports":[{"port":80,"nodePort":30080}]}}'

# Delete service
kubectl delete svc <service-name>
```

---

## Service YAML Examples

### ClusterIP Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-svc
spec:
  type: ClusterIP  # Default, can be omitted
  selector:
    app: backend
  ports:
  - port: 80
    targetPort: 8080
```

### NodePort Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-nodeport
spec:
  type: NodePort
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30080
```

### LoadBalancer Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: public-web
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 8080
```

### Headless Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql-headless
spec:
  clusterIP: None  # Headless
  selector:
    app: mysql
  ports:
  - port: 3306
    targetPort: 3306
```

### ExternalName Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: external-db
spec:
  type: ExternalName
  externalName: database.company.com
```

### Multi-Port Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: multi-port-svc
spec:
  selector:
    app: api
  ports:
  - name: http
    port: 80
    targetPort: 8080
  - name: https
    port: 443
    targetPort: 8443
```

### Service with Session Affinity
```yaml
apiVersion: v1
kind: Service
metadata:
  name: sticky-svc
spec:
  selector:
    app: session-app
  sessionAffinity: ClientIP
  sessionAffinityConfig:
    clientIP:
      timeoutSeconds: 10800
  ports:
  - port: 80
    targetPort: 8080
```

### Service Without Selector
```yaml
apiVersion: v1
kind: Service
metadata:
  name: legacy-app
spec:
  ports:
  - port: 80
    targetPort: 8080
---
apiVersion: v1
kind: Endpoints
metadata:
  name: legacy-app  # Must match service name
subsets:
- addresses:
  - ip: 10.0.0.5
  ports:
  - port: 8080
```

### Service with Multiple Selectors
```yaml
apiVersion: v1
kind: Service
metadata:
  name: prod-web-svc
spec:
  selector:
    app: web
    env: production
  ports:
  - port: 80
    targetPort: 8080
```

---

## DNS Testing

### Test DNS Resolution
```bash
# Create test pod
kubectl run dnstest --rm -it --image=busybox -- sh

# Inside pod:
nslookup <service-name>
nslookup <service-name>.<namespace>
nslookup <service-name>.<namespace>.svc.cluster.local

# Test connectivity
wget -O- <service-name>
wget -O- <service-name>.<namespace>
```

### DNS Record Types

**A Record (ClusterIP services):**
```
<service>.<namespace>.svc.cluster.local → ClusterIP
```

**A Records (Headless services):**
```
<service>.<namespace>.svc.cluster.local → Pod IPs
<pod-name>.<service>.<namespace>.svc.cluster.local → Specific Pod IP
```

**SRV Records:**
```
_<port-name>._<protocol>.<service>.<namespace>.svc.cluster.local
```

---

## Troubleshooting Services

### Check Service Configuration
```bash
# Verify service exists
kubectl get svc <service-name>

# Check selector matches pods
kubectl describe svc <service-name>
kubectl get pods --show-labels

# Verify endpoints
kubectl get endpoints <service-name>
# Should show pod IPs - if empty, selector doesn't match
```

### Common Issues

**No Endpoints:**
- Service selector doesn't match pod labels
- No pods are running/ready
- Pods not in same namespace

**Can't Access Service:**
- Check NetworkPolicies blocking traffic
- Verify pod readiness probes passing
- Check service port vs targetPort

**DNS Not Resolving:**
- CoreDNS pods not running
- Check kube-dns service in kube-system
- Verify pod's /etc/resolv.conf

### Debug Commands
```bash
# Check CoreDNS
kubectl get pods -n kube-system -l k8s-app=kube-dns

# Test DNS from pod
kubectl run test --rm -it --image=busybox -- nslookup kubernetes.default

# Check service endpoints
kubectl get endpoints <service-name>

# Describe service for events
kubectl describe svc <service-name>

# Test connectivity
kubectl run curl --rm -it --image=curlimages/curl -- curl http://<service>:<port>
```

---

## Exam Tips

1. **Use imperative commands** for speed: `kubectl expose deployment ...`
2. **Remember DNS naming:** `<service>.<namespace>.svc.cluster.local`
3. **Check endpoints:** If service not working, verify `kubectl get endpoints`
4. **Selector must match:** Service selector must match pod labels exactly
5. **Port vs TargetPort:**
   - `port`: Service port (what clients use)
   - `targetPort`: Pod port (where container listens)
6. **Headless = clusterIP: None**
7. **NodePort range:** 30000-32767
8. **Test DNS:** Use `nslookup` or `wget` from test pods
9. **Session affinity:** Only `ClientIP` is supported
10. **Service names:** Must be valid DNS names (lowercase, no underscores)

---

## Practice Strategy

1. Create Deployments with labels
2. Expose with different Service types
3. Test DNS resolution from pods
4. Practice cross-namespace communication
5. Troubleshoot missing endpoints
6. Create headless services for StatefulSets
7. Use port-forward for local testing
8. Practice imperative commands for speed

Good luck with your CKAD preparation!
