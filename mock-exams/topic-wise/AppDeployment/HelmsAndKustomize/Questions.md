# CKAD Helm and Kustomize Practice Questions

## Application Deployment (20%)

### Helm Package Manager and Kustomize

---

## Helm Questions

### Question 1: Add Helm Repository
Add the Bitnami Helm repository with URL `https://charts.bitnami.com/bitnami` and name `bitnami`.

---

### Question 2: Search Helm Charts
Search for nginx charts in the Bitnami repository.

---

### Question 3: Install Helm Chart
Install the `bitnami/nginx` chart with release name `my-nginx` in the `default` namespace.

---

### Question 4: List Helm Releases
List all Helm releases in the current namespace.

---

### Question 5: Upgrade Helm Release
Upgrade the `my-nginx` release to set the replica count to 3.

---

### Question 6: Helm Release History
View the revision history of the `my-nginx` release.

---

### Question 7: Rollback Helm Release
Rollback the `my-nginx` release to the previous version.

---

### Question 8: Uninstall Helm Release
Uninstall the `my-nginx` Helm release.

---

### Question 9: Install with Custom Values
Install `bitnami/mysql` with release name `my-db` and set the root password to `secretpassword`.

---

### Question 10: Install with Values File
Create a values file that sets `replicaCount: 2` and `service.type: NodePort`, then install nginx using this file.

---

### Question 11: Get Helm Values
Get the values used in the `my-nginx` release.

---

### Question 12: Helm Release Status
Check the status of the `my-nginx` Helm release.

---

### Question 13: Install Specific Chart Version
Install `bitnami/postgresql` version `12.1.0` with release name `my-postgres`.

---

### Question 14: Dry Run Installation
Perform a dry-run installation of `bitnami/redis` to see what resources would be created without actually installing.

---

### Question 15: Helm Template
Generate the Kubernetes manifests for `bitnami/nginx` without installing it.

---

## Kustomize Questions

### Question 16: Create Base Kustomization
Create a base kustomization with a Deployment and Service for nginx.

---

### Question 17: Apply Kustomization
Apply a kustomization from the current directory.

---

### Question 18: Kustomize Build
Build and view the output of a kustomization without applying it.

---

### Question 19: Overlay for Production
Create a production overlay that changes the replica count to 5 and adds a `env=production` label.

---

### Question 20: Patch with Kustomize
Use Kustomize to patch a Deployment to add an environment variable `APP_ENV=staging`.

---

### Question 21: ConfigMap Generator
Use Kustomize to generate a ConfigMap from literal values `db_host=localhost` and `db_port=5432`.

---

### Question 22: Secret Generator
Use Kustomize to generate a Secret from literal value `password=mysecret`.

---

### Question 23: Multiple Overlays
Create two overlays (dev and prod) with different replica counts and resource limits.

---

### Question 24: Name Prefix
Use Kustomize to add a prefix `prod-` to all resource names.

---

### Question 25: Common Labels
Use Kustomize to add common labels `team=backend` and `project=api` to all resources.

---

## Key Concepts

### Helm Basics

**Helm** is a package manager for Kubernetes that:
- Uses **Charts** (packaged Kubernetes resources)
- Manages **Releases** (installed instances of charts)
- Supports **templating** and **values** for customization
- Provides **version control** and **rollback** capabilities

**Key Components:**
- **Chart:** Package of Kubernetes resources
- **Release:** Instance of a chart running in cluster
- **Repository:** Collection of charts
- **Values:** Configuration parameters for charts

### Kustomize Basics

**Kustomize** is a template-free way to customize Kubernetes configurations:
- Uses **overlays** to modify base configurations
- Built into kubectl (kubectl apply -k)
- No templating - pure YAML
- **Patches** for modifications
- **Generators** for ConfigMaps and Secrets

**Key Concepts:**
- **Base:** Original/common Kubernetes resources
- **Overlay:** Environment-specific modifications
- **kustomization.yaml:** Defines customizations
- **Patches:** Modify existing resources
- **Generators:** Create ConfigMaps/Secrets

---

## Essential Helm Commands

### Repository Management
```bash
# Add repository
helm repo add <name> <url>

# List repositories
helm repo list

# Update repositories
helm repo update

# Remove repository
helm repo remove <name>
```

### Search Charts
```bash
# Search in all repos
helm search repo <keyword>

# Search in specific repo
helm search repo bitnami/nginx

# Search Helm Hub
helm search hub <keyword>
```

### Install Charts
```bash
# Basic install
helm install <release-name> <chart>

# Install with custom values
helm install <release-name> <chart> --set key=value

# Install with values file
helm install <release-name> <chart> -f values.yaml

# Install in specific namespace
helm install <release-name> <chart> -n <namespace>

# Install specific version
helm install <release-name> <chart> --version <version>

# Dry run
helm install <release-name> <chart> --dry-run --debug

# Generate manifests without installing
helm template <release-name> <chart>
```

### Manage Releases
```bash
# List releases
helm list
helm list -n <namespace>
helm list --all-namespaces

# Get release info
helm status <release-name>

# Get release values
helm get values <release-name>

# Get release manifest
helm get manifest <release-name>

# Get all release info
helm get all <release-name>
```

### Upgrade Releases
```bash
# Upgrade release
helm upgrade <release-name> <chart>

# Upgrade with new values
helm upgrade <release-name> <chart> --set key=value

# Upgrade with values file
helm upgrade <release-name> <chart> -f values.yaml

# Install if not exists, upgrade if exists
helm upgrade --install <release-name> <chart>
```

### Rollback Releases
```bash
# View history
helm history <release-name>

# Rollback to previous version
helm rollback <release-name>

# Rollback to specific revision
helm rollback <release-name> <revision>
```

### Uninstall Releases
```bash
# Uninstall release
helm uninstall <release-name>

# Uninstall and keep history
helm uninstall <release-name> --keep-history
```

---

## Essential Kustomize Commands

### Basic Usage
```bash
# Apply kustomization from directory
kubectl apply -k <directory>

# Build kustomization (view output)
kubectl kustomize <directory>

# Delete resources from kustomization
kubectl delete -k <directory>
```

### With Kustomize Binary
```bash
# Build kustomization
kustomize build <directory>

# Apply with kustomize binary
kustomize build <directory> | kubectl apply -f -
```

---

## Helm Examples

### Basic Installation
```bash
# Add repo
helm repo add bitnami https://charts.bitnami.com/bitnami

# Update repos
helm repo update

# Install nginx
helm install my-nginx bitnami/nginx

# Check status
helm status my-nginx

# List releases
helm list
```

### Installation with Custom Values
```bash
# Install with --set
helm install my-nginx bitnami/nginx \
  --set replicaCount=3 \
  --set service.type=NodePort

# Install with values file
cat > values.yaml << EOF
replicaCount: 3
service:
  type: NodePort
  nodePorts:
    http: 30080
EOF

helm install my-nginx bitnami/nginx -f values.yaml
```

### Upgrade and Rollback
```bash
# Upgrade
helm upgrade my-nginx bitnami/nginx --set replicaCount=5

# View history
helm history my-nginx

# Rollback
helm rollback my-nginx

# Rollback to specific revision
helm rollback my-nginx 2
```

### Values File Example
```yaml
# values.yaml
replicaCount: 2

image:
  repository: nginx
  tag: "1.24"
  pullPolicy: IfNotPresent

service:
  type: LoadBalancer
  port: 80

resources:
  limits:
    cpu: 200m
    memory: 256Mi
  requests:
    cpu: 100m
    memory: 128Mi

ingress:
  enabled: true
  hosts:
    - host: myapp.example.com
      paths:
        - path: /
          pathType: Prefix
```

### Install with Multiple Values Files
```bash
# Base values + environment-specific
helm install my-app bitnami/nginx \
  -f values-base.yaml \
  -f values-production.yaml
```

---

## Kustomize Examples

### Directory Structure
```
app/
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
└── overlays/
    ├── dev/
    │   └── kustomization.yaml
    └── prod/
        └── kustomization.yaml
```

### Base Configuration

**base/deployment.yaml:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
```

**base/service.yaml:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 80
```

**base/kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- deployment.yaml
- service.yaml
```

### Production Overlay

**overlays/prod/kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
- ../../base

namePrefix: prod-

commonLabels:
  env: production

replicas:
- name: myapp
  count: 5

patches:
- patch: |-
    - op: replace
      path: /spec/template/spec/containers/0/resources
      value:
        requests:
          cpu: 200m
          memory: 256Mi
        limits:
          cpu: 500m
          memory: 512Mi
  target:
    kind: Deployment
    name: myapp
```

**Apply:**
```bash
kubectl apply -k overlays/prod
```

### ConfigMap Generator

**kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

configMapGenerator:
- name: app-config
  literals:
  - DB_HOST=localhost
  - DB_PORT=5432
  - APP_ENV=production

resources:
- deployment.yaml
```

### Secret Generator

**kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

secretGenerator:
- name: db-secret
  literals:
  - username=admin
  - password=secretpass
  type: Opaque

resources:
- deployment.yaml
```

### Strategic Merge Patch

**kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
- ../base

patchesStrategicMerge:
- patch-deployment.yaml
```

**patch-deployment.yaml:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: nginx
        env:
        - name: ENV
          value: production
```

### JSON Patch

**kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
- ../base

patches:
- target:
    kind: Deployment
    name: myapp
  patch: |-
    - op: replace
      path: /spec/replicas
      value: 5
    - op: add
      path: /spec/template/spec/containers/0/env
      value:
        - name: ENV
          value: production
```

### Common Labels and Annotations

**kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

commonLabels:
  app: myapp
  team: backend
  env: production

commonAnnotations:
  version: "1.0"
  managed-by: kustomize

resources:
- deployment.yaml
- service.yaml
```

### Name Prefix/Suffix

**kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namePrefix: staging-
nameSuffix: -v2

resources:
- deployment.yaml
- service.yaml
```

Result: `staging-myapp-v2`

### Images Transformer

**kustomization.yaml:**
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- deployment.yaml

images:
- name: nginx
  newTag: 1.25
  # Or change image entirely:
  # newName: myregistry.com/nginx
  # newTag: custom
```

---

## Troubleshooting

### Helm Issues

**Chart not found:**
```bash
# Update repos
helm repo update

# Search for chart
helm search repo <chart-name>

# Check repo list
helm repo list
```

**Release already exists:**
```bash
# List releases
helm list --all

# Uninstall old release
helm uninstall <release-name>

# Or use --replace flag
helm install <release-name> <chart> --replace
```

**Values not applying:**
```bash
# Check current values
helm get values <release-name>

# Check all values (including defaults)
helm get values <release-name> --all

# Verify with dry-run
helm upgrade <release-name> <chart> -f values.yaml --dry-run --debug
```

### Kustomize Issues

**Resources not found:**
```bash
# Check kustomization.yaml resources list
cat kustomization.yaml

# Verify files exist
ls -la
```

**Patches not applying:**
```bash
# Build and view output
kubectl kustomize .

# Check patch syntax
# Strategic merge: YAML format
# JSON patch: JSON patch format with op, path, value
```

**Base path issues:**
```bash
# In overlay kustomization.yaml
bases:
- ../../base  # Correct relative path

# Or use resources (newer syntax)
resources:
- ../../base
```

---

## Exam Tips

### Helm
1. **Know basic commands:** install, upgrade, rollback, uninstall
2. **--set vs -f:** Use --set for single values, -f for multiple
3. **helm template:** Generate manifests without installing
4. **helm repo add:** Add repo before searching/installing
5. **Quick install:** `helm install name repo/chart`
6. **Version specific:** Use `--version` flag
7. **Namespace:** Use `-n` flag for non-default namespaces
8. **Rollback:** `helm rollback name` goes to previous
9. **History:** `helm history name` shows revisions
10. **Status:** `helm status name` shows current state

### Kustomize
1. **Built into kubectl:** Use `kubectl apply -k` or `kubectl kustomize`
2. **Directory structure:** base/ and overlays/
3. **kustomization.yaml:** Required in each directory
4. **Build first:** Use `kubectl kustomize .` to preview
5. **Generators:** ConfigMap and Secret generators
6. **Patches:** Strategic merge or JSON patch
7. **Common labels:** Easy way to add labels to all resources
8. **Name prefix/suffix:** Add to all resource names
9. **Images:** Change image tags without editing YAMLs
10. **Relative paths:** Use correct paths in bases/resources

---

## Common Patterns

### Helm: Install with Overrides
```bash
helm install myapp bitnami/nginx \
  --set replicaCount=3 \
  --set service.type=LoadBalancer \
  --namespace production \
  --create-namespace
```

### Helm: Upgrade or Install
```bash
helm upgrade --install myapp bitnami/nginx \
  -f values.yaml \
  --namespace production
```

### Kustomize: Multi-Environment
```bash
# Dev
kubectl apply -k overlays/dev

# Staging
kubectl apply -k overlays/staging

# Production
kubectl apply -k overlays/prod
```

### Kustomize: ConfigMap from File
```yaml
configMapGenerator:
- name: app-config
  files:
  - config.properties
  - application.yaml
```

---

## Practice Strategy

### For Helm:
1. Add bitnami repo and install various charts
2. Practice upgrades with different values
3. Rollback to previous versions
4. Use helm template to see generated manifests
5. Create custom values files
6. Practice --set for quick overrides

### For Kustomize:
1. Create base configurations
2. Build dev/staging/prod overlays
3. Practice patches (both strategic merge and JSON)
4. Use ConfigMap and Secret generators
5. Add common labels and name prefixes
6. Build without applying to preview changes

Good luck with your CKAD preparation!
