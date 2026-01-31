Trivy is a simple, fast security scanner used to find vulnerabilities, misconfigurations, secrets, and license issues — especially popular in Docker, Kubernetes, and CI/CD setups. <br>

**“security scan before things break in prod”**

## What Trivy scans:

### 1. Container images
Scans OS libraries + language dependency inside image

```
trivy image nginx:latest
```
OR with docker:
```
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image nginx:latest
```
You can use `image --severity HIGH,CRITICAL` <br>

### 2. Scan file system and repos:

Scan source code directories.
```
trivy fs .
# scan only secret
trivy fs --scanners secret .
```
OR with docker:
```
docker run --rm \
  -v microservices-poc:/workspace \
  aquasec/trivy:latest \
  fs /workspace
```

### 3. Scan Kubernetes YAML / Helm / IaC
```
trivy config .
```

With Docker:
```
docker run --rm   -v k8s:/workspace   aquasec/trivy:latest   config /workspace
```

### 4. Scan a Running Kubernetes Cluster
```
## scan kubernetes cluster
trivy k8s cluster
### summary view
trivy k8s --report summary cluster
```
### 5. Scan a Specific Kubernetes Namespace
```
trivy k8s namespace my-namespace
```


## Ignore known vulnerabilities (real-world need)

Create `.trivyignore`:
```
CVE-2023-12345
CVE-2022-99999
```

### Run scan:
```
trivy image nginx:latest
```

Trivy in CICD
```
trivy_scan:
  image: aquasec/trivy:latest
  script:
    - trivy image --exit-code 1 --severity CRITICAL,HIGH myapp:latest
```

Best Practice Combo:

| Stage       | Tool          |
| ----------- | ------------- |
| Code commit | Trivy fs      |
| Image build | Trivy image   |
| Pre-deploy  | Trivy config  |
| Runtime     | Falco         |
| Policy      | Kyverno / OPA |
