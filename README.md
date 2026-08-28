# 🚀 DevSecOps CI/CD Pipeline — Google Online Boutique

A production-inspired **DevSecOps, CI/CD, GitOps, Kubernetes, and Observability** project built around Google's **Online Boutique** microservices application.

The project demonstrates an end-to-end software delivery lifecycle:

**Source Code → Security → Testing → Code Quality → Container Security → Container Registry → GitOps → Kubernetes → Monitoring**

---

## 🏗️ Project Architecture

```text
                         ┌─────────────────────┐
                         │       GitHub        │
                         │    Source Code      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                       ┌────────────────────────┐
                       │ Jenkins Multibranch     │
                       │       Pipeline          │
                       └────────────┬───────────┘
                                    │
             ┌──────────────────────┼──────────────────────┐
             │                      │                      │
             ▼                      ▼                      ▼
        GitLeaks               Unit Tests             SonarQube
    Secret Detection          Code Testing          Code Analysis
                                                            │
                                                            ▼
                                                     Quality Gate
                                                            │
                                                            ▼
                                                       Trivy FS
                                                            │
                                                            ▼
                                                   Docker Image
                                                            │
                                                            ▼
                                                     Trivy Image
                                                            │
                                                            ▼
                                                     Docker Hub
                                                            │
                                                            ▼
                         ┌────────────────────────────────────────┐
                         │ Kubernetes Manifest Repository        │
                         └──────────────────┬─────────────────────┘
                                            │
                                            ▼
                                         Argo CD
                                            │
                                            ▼
                         ┌────────────────────────────────────────┐
                         │         Kubernetes Cluster             │
                         │              kubeadm                   │
                         └──────────────────┬─────────────────────┘
                                            │
                          ┌─────────────────┼─────────────────┐
                          │                 │                 │
                          ▼                 ▼                 ▼
                    Microservices        RBAC            Monitoring
                                      Least Privilege        │
                          │                                  │
                   ┌──────┼──────┐                   ┌──────┴──────┐
                   ▼      ▼      ▼                   ▼             ▼
              ServiceAccount Role RoleBinding     Prometheus  kube-state-metrics
                                                            │
                                                            ▼
                                                         Grafana
```

---

# ⭐ Main Features

- 🔄 Jenkins **Multibranch CI/CD Pipeline**
- 🌿 Branch-based microservice identification
- 🔐 **GitLeaks** secret detection
- 🧪 Automated unit testing
- 📊 **SonarQube** static code analysis
- 🚦 **SonarQube Quality Gate**
- 🛡️ **Trivy filesystem vulnerability scanning**
- 🐳 **Trivy container image scanning**
- 📦 Docker Hub container registry
- 🏷️ Build-specific Docker image versioning
- 🔀 Separate Kubernetes Manifest Repository
- 🔁 **GitOps deployment using Argo CD**
- ☸️ Kubernetes cluster built with **kubeadm**
- 🔑 Kubernetes **ServiceAccounts**
- 🛂 Kubernetes **Role & RoleBinding**
- 🔒 **Principle of Least Privilege**
- 📈 **Prometheus** monitoring
- ☸️ **kube-state-metrics**
- 📊 **Grafana dashboards**
- 🔄 Git-based deployment history
- 🌐 Polyglot microservices architecture

---

# 🔄 End-to-End Pipeline

The Jenkins pipeline follows this workflow:

```text
Developer
    │
    ▼
GitHub
    │
    ▼
Git Clone
    │
    ▼
GitLeaks
    │
    ▼
Unit Tests
    │
    ▼
SonarQube Analysis
    │
    ▼
SonarQube Quality Gate
    │
    ▼
Trivy Filesystem Scan
    │
    ▼
Docker Image Tag
    │
    ▼
Trivy Image Scan
    │
    ▼
Docker Hub
    │
    ▼
Update Kubernetes Manifest
    │
    ▼
Git Push
    │
    ▼
Argo CD
    │
    ▼
Kubernetes
    │
    ▼
Prometheus + kube-state-metrics
    │
    ▼
Grafana
```

Each security and quality stage can prevent the pipeline from progressing when configured checks fail.

---

# 🔐 DevSecOps

Security is integrated directly into the CI/CD lifecycle rather than being performed only after deployment.

```text
              Source Code
                   │
                   ▼
                GitLeaks
                   │
                   ▼
               Unit Tests
                   │
                   ▼
              SonarQube
                   │
                   ▼
             Quality Gate
                   │
                   ▼
                Trivy FS
                   │
                   ▼
             Docker Image
                   │
                   ▼
              Trivy Image
                   │
                   ▼
             Docker Hub
                   │
                   ▼
               Argo CD
                   │
                   ▼
              Kubernetes
```

## Security Tools

| Tool | Purpose |
|---|---|
| **GitLeaks** | Detect accidentally committed secrets, credentials and tokens |
| **SonarQube** | Static code quality and security analysis |
| **SonarQube Quality Gate** | Decide whether code meets defined quality/security requirements |
| **Trivy FS** | Scan project files and dependencies for vulnerabilities |
| **Trivy Image** | Scan the actual container image for vulnerabilities |
| **Jenkins Credentials** | Securely store CI/CD credentials |
| **Kubernetes RBAC** | Control access to Kubernetes resources |
| **Least Privilege** | Grant only the permissions actually required |

---

# 📊 SonarQube

SonarQube performs static analysis of application source code.

It helps identify:

- Bugs
- Code smells
- Security issues
- Maintainability problems
- Duplicated code
- Other language-specific code quality issues

The project contains multiple programming languages, so SonarQube can be used according to the requirements of each microservice.

For example, Java services are analyzed with their compiled classes available to SonarQube.

```text
Source Code
     │
     ▼
SonarQube Analysis
     │
     ▼
Quality Metrics
     │
     ▼
Quality Gate
     │
     ├── PASS → Continue
     │
     └── FAIL → Stop Pipeline
```

---

# 🛡️ Trivy

Trivy is used for vulnerability detection.

Two different scans are used in the pipeline.

### Filesystem Scan

```text
Project Files
     │
     ▼
  Trivy FS
     │
     ▼
Dependencies / Packages
     │
     ▼
Vulnerability Results
```

Example:

```bash
trivy fs --severity HIGH,CRITICAL --exit-code 1 .
```

### Container Image Scan

```text
Docker Image
     │
     ▼
Trivy Image
     │
     ▼
OS Packages
Application Dependencies
Libraries
     │
     ▼
Vulnerability Results
```

Example:

```bash
trivy image --severity HIGH,CRITICAL --exit-code 1 <image>
```

Using both scans provides security coverage at different points in the software delivery lifecycle.

---

# 🧪 Testing

Unit testing is performed before the application proceeds toward container deployment.

Different services may use different testing frameworks because the application is polyglot.

Examples:

| Language | Typical Test Tool |
|---|---|
| Java | Gradle / JUnit |
| Go | `go test` |
| Python | `pytest` |
| Node.js | `npm test` |
| C# | `dotnet test` |

For Gradle-based services:

```bash
gradle test
```

---

# 🐳 Docker

The project uses Docker for containerized microservices.

Images are versioned using the Jenkins build number.

Example:

```text
ozairkhan1/adservice:v21
ozairkhan1/adservice:v22
ozairkhan1/adservice:v23
```

The image tag is generated dynamically:

```text
IMAGE_TAG = v${BUILD_NUMBER}
```

This provides traceability between a Jenkins build and the container deployed to Kubernetes.

> The current project intentionally tags an existing locally available `:latest` image rather than rebuilding the image inside this pipeline.

Example:

```bash
docker tag ozairkhan1/adservice:latest ozairkhan1/adservice:v25
```

The resulting versioned image is then scanned and pushed to Docker Hub.

---

# 📦 Docker Hub

Docker Hub acts as the container registry.

The pipeline pushes versioned images such as:

```text
ozairkhan1/adservice:v25
ozairkhan1/frontend:v18
ozairkhan1/cartservice:v12
```

The versioned image is then referenced by the Kubernetes manifest.

---

# 🌿 Jenkins Multibranch Pipeline

The pipeline is designed to support multiple microservices using a common Jenkinsfile.

The microservice name is determined from the branch:

```groovy
SERVICE = "${env.BRANCH_NAME}"
```

For example:

```text
Branch:
adservice

        ↓

SERVICE:
adservice

        ↓

Docker Image:
ozairkhan1/adservice:v25
```

This allows the same CI/CD logic to be reused across multiple microservices.

---

# 🧰 Jenkins Tools

The Jenkins pipeline uses centrally configured tools such as:

```text
JDK
Gradle
SonarQube Scanner
```

Tools are configured in Jenkins under:

```text
Manage Jenkins
    ↓
Tools
```

The pipeline can then use the configured installations rather than hardcoding installation paths.

---

# 🔀 Git Repositories

The project separates application source code from Kubernetes deployment configuration.

## Application Repository

```text
DevSecOps
```

Contains:

```text
Microservices
Source Code
Dockerfiles
Jenkinsfile
CI/CD configuration
```

## Kubernetes Manifest Repository

```text
Kubernetes-ManifestFiles
```

Contains:

```text
11-Microservices-Manifests
```

This separation supports the GitOps deployment model.

---

# 🔁 GitOps with Argo CD

Argo CD is responsible for deploying the desired Kubernetes state from Git.

The workflow is:

```text
Jenkins
   │
   ▼
Docker Image
   │
   ▼
Docker Hub
   │
   ▼
Update Manifest Repository
   │
   ▼
Git Commit
   │
   ▼
Argo CD
   │
   ▼
Kubernetes
```

For example, Jenkins updates:

```yaml
image: ozairkhan1/adservice:v25
```

in the Kubernetes manifest repository.

Argo CD detects the Git change and synchronizes the Kubernetes cluster.

This follows the GitOps principle:

> **Git is the source of truth for the desired Kubernetes state.**

---

# ☸️ Kubernetes

The application runs on a Kubernetes cluster created using **kubeadm**.

The cluster consists of:

```text
Kubernetes Cluster
│
├── Control Plane
│   ├── API Server
│   ├── Scheduler
│   ├── Controller Manager
│   └── etcd
│
└── Worker Nodes
    ├── Online Boutique Services
    └── Monitoring Components
```

The microservices are deployed as Kubernetes workloads and managed through Kubernetes resources.

---

# 🔑 Kubernetes RBAC

The project follows the **Principle of Least Privilege** when providing Kubernetes access.

Instead of giving workloads or automation accounts unnecessary:

```text
cluster-admin
```

permissions are restricted using:

```text
ServiceAccount
      │
      ▼
RoleBinding
      │
      ▼
Role
      │
      ▼
Allowed Resources + Verbs
```

## ServiceAccount

Provides an identity for a workload or automation process.

## Role

Defines which resources and actions are permitted within a namespace.

## RoleBinding

Connects the ServiceAccount to the Role.

This allows permissions to be granted precisely rather than giving broad cluster-wide access.

---

# 🔒 Principle of Least Privilege

The project follows:

> **Give an identity only the permissions required to perform its task.**

For example, if a process only needs to read Pods:

```text
Resource:
pods

Verbs:
get
list
watch
```

It should not receive unrestricted permissions over the entire cluster.

This reduces the potential impact of a compromised workload or credential.

---

# 📈 Monitoring & Observability

The Kubernetes environment is monitored using:

```text
Prometheus
    +
kube-state-metrics
    +
Grafana
```

Architecture:

```text
                  Kubernetes
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
      Kubelet     Node Metrics   kube-state-metrics
          │                       │
          └───────────┬───────────┘
                      ▼
                  Prometheus
                      │
                      ▼
                   Grafana
```

---

# 📡 Prometheus

Prometheus collects and stores time-series metrics.

It can monitor:

- Cluster resources
- Node resources
- Pod metrics
- Container metrics
- Kubernetes object metrics
- Application metrics where exposed

Prometheus periodically scrapes configured metrics endpoints.

---

# ☸️ kube-state-metrics

kube-state-metrics exposes metrics about the state of Kubernetes objects.

Examples include:

- Pods
- Deployments
- ReplicaSets
- StatefulSets
- DaemonSets
- Nodes
- Jobs
- Namespaces

It can provide information such as:

```text
Desired replicas
Available replicas
Pod state
Deployment state
Node state
```

kube-state-metrics complements infrastructure and resource metrics by exposing the state of Kubernetes objects.

---

# 📊 Grafana

Grafana provides visualization for the metrics collected by Prometheus.

Dashboards can be used to monitor:

- CPU utilization
- Memory utilization
- Node health
- Pod status
- Pod restarts
- Deployment status
- Cluster resources
- Kubernetes object state
- Application metrics

The monitoring flow is:

```text
Kubernetes
    │
    ▼
Prometheus
    │
    ▼
Grafana
    │
    ▼
Monitoring Dashboards
```

---

# 🔄 Complete Deployment Lifecycle

```text
┌──────────────────────┐
│     Developer        │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│       GitHub         │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Jenkins Multibranch  │
└──────────┬───────────┘
           │
           ▼
      Git Clone
           │
           ▼
       GitLeaks
           │
           ▼
      Unit Tests
           │
           ▼
      SonarQube
           │
           ▼
     Quality Gate
           │
           ▼
       Trivy FS
           │
           ▼
    Docker Image Tag
           │
           ▼
      Trivy Image
           │
           ▼
       Docker Hub
           │
           ▼
 Update Manifest Repo
           │
           ▼
      Git Commit
           │
           ▼
        Argo CD
           │
           ▼
     Kubernetes
           │
           ▼
 Prometheus + KSM
           │
           ▼
        Grafana
```

---

# 🔐 Credentials & Secrets

Credentials are managed through Jenkins Credentials rather than being hardcoded in the Jenkinsfile.

Examples include:

```text
Git credentials
Docker Hub credentials
SonarQube authentication
Kubernetes credentials
```

Sensitive credentials should never be committed to Git.

GitLeaks provides an additional layer of protection by detecting secrets that may accidentally be committed.

---

# 🎯 Project Objectives

This project demonstrates practical implementation of:

- **DevSecOps**
- **Continuous Integration**
- **Continuous Deployment**
- **GitOps**
- **Microservices**
- **Containerization**
- **Container Security**
- **Static Code Analysis**
- **Kubernetes Administration**
- **Kubernetes RBAC**
- **Least Privilege**
- **Infrastructure Monitoring**
- **Observability**
- **Automated Deployment**

---

# 🧰 Technology Stack

| Category | Technology |
|---|---|
| Source Control | Git / GitHub |
| CI/CD | Jenkins |
| Pipeline | Jenkins Multibranch Pipeline |
| Shared Library | Jenkins Shared Library |
| Secret Detection | GitLeaks |
| Code Analysis | SonarQube |
| Quality Gate | SonarQube |
| Vulnerability Scanning | Trivy |
| Containerization | Docker |
| Container Registry | Docker Hub |
| Orchestration | Kubernetes |
| Cluster Installation | kubeadm |
| GitOps | Argo CD |
| Metrics | Prometheus |
| Kubernetes Metrics | kube-state-metrics |
| Visualization | Grafana |
| Authentication | Kubernetes ServiceAccount |
| Authorization | Kubernetes RBAC |
| Access Control | Role / RoleBinding |
| Build | Gradle / Language-specific tools |

---

# 🌐 Online Boutique Microservices

The project is based on Google's Online Boutique microservices application.

The application contains multiple services implemented using different technologies, making it useful for demonstrating a polyglot DevSecOps pipeline.

The pipeline is designed so that each microservice can be processed independently through the Jenkins Multibranch Pipeline.

---

# 🧠 DevSecOps Principles Demonstrated

### Shift-Left Security

Security checks are performed before deployment:

```text
Code
 ↓
GitLeaks
 ↓
Testing
 ↓
SonarQube
 ↓
Quality Gate
 ↓
Trivy
 ↓
Deployment
```

### Immutable Versioned Artifacts

Images use build-specific tags instead of relying solely on `latest`.

### GitOps

Kubernetes desired state is stored in Git.

### Least Privilege

Kubernetes permissions are explicitly restricted through RBAC.

### Automation

The application delivery process is automated from source code through deployment.

### Observability

Prometheus, kube-state-metrics, and Grafana provide visibility into the running Kubernetes environment.

---

# 📌 Project Summary

This project combines modern DevSecOps and cloud-native technologies into a complete delivery platform:

```text
GitHub
   ↓
Jenkins
   ↓
GitLeaks
   ↓
Unit Tests
   ↓
SonarQube
   ↓
Quality Gate
   ↓
Trivy
   ↓
Docker
   ↓
Docker Hub
   ↓
GitOps
   ↓
Argo CD
   ↓
Kubernetes / kubeadm
   ↓
RBAC / Least Privilege
   ↓
Prometheus
   ↓
kube-state-metrics
   ↓
Grafana
```

The result is an automated pipeline that integrates **code quality, security, containerization, GitOps deployment, Kubernetes security, and observability** into a single end-to-end DevSecOps workflow.

---

# 👨‍💻 Author

**Ozair Khan**

DevOps | Cloud | Networking & Security

GitHub: https://github.com/OzairKhan1
