# Jenkins CI/CD Pipeline & Automation Notes
---

# 1. Introduction to Jenkins
---

## Definition

**Jenkins** is a widely-adopted, **open-source automation server** that fundamentally powers **Continuous Integration** and **Continuous Delivery** workflows across modern software teams. It natively handles the entire lifecycle of code — from **building** and **testing** through **packaging** and **deploying** — without manual intervention at any stage.

*"Jenkins is the backbone engine that stitches together every phase of software delivery into one seamless, automated pipeline."*

* **Open-Source** → freely available with a massive global community driving its evolution
* **Automation Server** → orchestrates repetitive build and deployment tasks automatically
* **CI/CD** → ensures that code changes are continuously validated, integrated, and shipped to production

---

# 2. Master-Agent Architecture
---

## Definition

Jenkins operates on a **Master-Agent architecture**, where the **Master node** serves as the central brain and the **Agent nodes** act as distributed workers that carry out the heavy lifting of actual build execution.

### Master Node Responsibilities

* **Job Scheduling** → determines when and how build jobs are triggered
* **Pipeline Management** → coordinates the overall flow of CI/CD stages
* **Build History Storage** → retains logs, reports, and historical records of every executed build

### Agent Node Responsibilities

* **Task Execution** → runs the actual compilation, testing, and packaging workloads
* **Environment Isolation** → each agent can host a distinct runtime (Java, Python, Node, etc.)
* **Scalability** → multiple agents can be spun up to handle parallel builds efficiently

```
┌──────────────────────────┐
│      JENKINS MASTER      │
│  (Schedules & Manages)   │
└────────────┬─────────────┘
             ↓
    ┌────────┴────────┐
    ↓                 ↓
┌──────────┐   ┌──────────┐
│  Agent 1 │   │  Agent 2 │
│ (Build)  │   │ (Test)   │
└──────────┘   └──────────┘
```

*"The master thinks and delegates; the agents do the actual work — this separation is what makes Jenkins horizontally scalable."*

---

# 3. Installation & Platform Support
---

## Definition

Jenkins is **platform-agnostic** and can be installed natively across virtually every major operating system and containerization platform available today.

* **Windows** → runs as a native Windows Service with GUI-based installer
* **Linux** → supports package managers like `apt`, `yum`, and `dnf` for seamless setup
* **macOS** → installable via Homebrew or standalone `.war` file execution
* **Docker** → heavily used for containerized, reproducible Jenkins environments
* **Kubernetes** → deployed as pods for cloud-native, elastic scaling of build infrastructure

```bash
# Running Jenkins instantly via Docker
docker run -d -p 8080:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts
```

---

# 4. Plugin Ecosystem
---

## Definition

The true power of Jenkins lies in its **plugin architecture** — a massive ecosystem of over **1,800+ community-maintained plugins** that extend its core functionality to integrate with virtually any tool in the DevOps landscape.

### Commonly Used Plugins

| Plugin Category | Plugin Name | Purpose |
|-----------------|-------------|---------|
| Source Control | **GitHub** | Repository integration, webhooks, PR triggers |
| Build Tools | **Maven** | Java project build automation and dependency management |
| Containerization | **Docker** | Build, push, and manage Docker images directly |
| Orchestration | **Kubernetes** | Dynamically provision build agents as K8s pods |
| Code Quality | **SonarQube** | Static analysis, code smell detection, quality gates |
| Notifications | **Slack** | Real-time build status alerts to team channels |
| Cloud (AWS) | **AWS Steps** | Interact with AWS services during pipeline execution |
| Cloud (Azure) | **Azure CLI** | Deploy and manage Azure resources from pipelines |

*"Plugins are what transform Jenkins from a simple automation engine into a fully customizable DevOps powerhouse."*

---

# 5. Security & Access Management
---

## Definition

Jenkins provides **robust, multi-layered security** mechanisms that control who can access the server, what actions they are permitted to perform, and how credentials are securely managed across pipelines.

### Authentication Methods

* **Local User Database** → Jenkins manages its own internal user accounts and passwords
* **LDAP Integration** → connects to enterprise directory services for centralized identity management
* **OAuth / SSO** → supports third-party identity providers like GitHub OAuth and Google SSO

### Authorization — Role-Based Access Control (RBAC)

* **Admin Role** → full system control including plugin management and global configuration
* **Developer Role** → can create, trigger, and monitor their own build jobs
* **Read-Only Role** → can only view build logs and pipeline status without modification rights

*"Security in Jenkins is not optional — it is a foundational layer that must be configured before any pipeline goes live."*

---

# 6. Freestyle Jobs vs Pipeline Jobs
---

## Definition

Jenkins offers two fundamentally different approaches to defining build jobs: **Freestyle Jobs** which are configured entirely through the web GUI, and **Pipeline Jobs** which represent the entire CI/CD workflow **as code** stored in a **Jenkinsfile**.

### Comparison Table

| Aspect | Freestyle Jobs | Pipeline Jobs |
|--------|---------------|---------------|
| **Configuration** | GUI-based, point-and-click | Code-based via Jenkinsfile |
| **Version Control** | Not stored in SCM | Stored in repository alongside code |
| **Complexity Handling** | Suitable for simple, single-step tasks | Built for complex, multi-stage workflows |
| **Maintainability** | Becomes unwieldy at scale | Cleanly scales with project growth |
| **Reusability** | Limited, each job configured independently | Shared libraries enable heavy reuse |
| **Visibility** | Basic build output view | Full stage-by-stage visual pipeline view |
| **Recommended For** | Quick prototypes and one-off tasks | Production-grade CI/CD implementations |

*"Freestyle jobs get you started quickly, but Pipeline jobs are where serious, production-level automation lives."*

---

# 7. Declarative vs Scripted Pipelines
---

## Definition

Pipeline jobs in Jenkins can be written in two distinct syntaxes: **Declarative Pipelines** which follow a rigid, structured format with predefined sections, and **Scripted Pipelines** which are powered by raw **Groovy** code offering maximum programmatic flexibility.

### Comparison Table

| Aspect | Declarative Pipeline | Scripted Pipeline |
|--------|---------------------|-------------------|
| **Syntax** | Structured, block-based with `pipeline {}` | Freeform Groovy inside `node {}` |
| **Readability** | Highly readable, even for beginners | Requires Groovy knowledge |
| **Flexibility** | Opinionated — follows fixed structure | Unrestricted — any Groovy logic allowed |
| **Error Handling** | Built-in `post {}` block for post-actions | Manual `try-catch-finally` required |
| **Validation** | Linted and validated before execution | No pre-execution validation |
| **Learning Curve** | Lower — ideal for teams new to Jenkins | Higher — suits advanced automation needs |
| **Maintenance** | Easier to maintain long-term | Can become complex and hard to debug |

---

# 8. Jenkinsfile — Pipeline as Code
---

## Definition

A **Jenkinsfile** is the heart of Pipeline-as-Code — a text file committed directly into the project repository that declaratively defines every **stage**, **step**, **environment variable**, **parameter**, and **deployment instruction** for the CI/CD workflow.

### Example Declarative Jenkinsfile

```groovy
pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'registry.example.com'
        APP_NAME        = 'my-application'
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
        booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Deploy after build?')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/org/repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh "docker build -t ${DOCKER_REGISTRY}/${APP_NAME}:${BUILD_NUMBER} ."
                sh "docker push ${DOCKER_REGISTRY}/${APP_NAME}:${BUILD_NUMBER}"
            }
        }

        stage('Deploy') {
            when {
                expression { params.DEPLOY == true }
            }
            steps {
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            slackSend message: "Build #${BUILD_NUMBER} PASSED ✅"
        }
        failure {
            slackSend message: "Build #${BUILD_NUMBER} FAILED ❌"
        }
        always {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            cleanWs()
        }
    }
}
```

### Key Jenkinsfile Sections

* **`agent`** → specifies where the pipeline or stage executes (any node, specific label, Docker container)
* **`environment`** → defines key-value pairs accessible throughout the pipeline
* **`parameters`** → allows runtime input when triggering the build manually
* **`stages`** → contains the ordered sequence of pipeline phases
* **`steps`** → the actual commands or actions executed within each stage
* **`post`** → actions that run after the pipeline completes (success, failure, always)

---

# 9. Pipeline Stages & Flow
---

## Definition

A Jenkins pipeline is fundamentally organized into **stages** — discrete, named phases that represent each logical step in the software delivery process, from source checkout all the way to production deployment.

### Standard Pipeline Stage Flow

```
┌─────────────────────────────────────────────────────────┐
│               JENKINS CI/CD PIPELINE FLOW               │
└─────────────────────────────────────────────────────────┘

  Developer Writes Code
             ↓
  Push to GitHub Repository
             ↓
  Webhook Triggers Jenkins Pipeline
             ↓
  ┌─────────────────────────┐
  │   Stage 1: CHECKOUT     │
  │   (Clone source code)   │
  └────────────┬────────────┘
               ↓
  ┌─────────────────────────┐
  │   Stage 2: BUILD        │
  │   (Compile application) │
  └────────────┬────────────┘
               ↓
  ┌─────────────────────────┐
  │   Stage 3: TEST         │
  │   (Run unit/int tests)  │
  └────────────┬────────────┘
               ↓
  ┌─────────────────────────┐
  │   Stage 4: PACKAGE      │
  │   (Create JAR/WAR/ZIP)  │
  └────────────┬────────────┘
               ↓
  ┌─────────────────────────┐
  │   Stage 5: DOCKER BUILD │
  │   (Build container img) │
  └────────────┬────────────┘
               ↓
  ┌─────────────────────────┐
  │   Stage 6: PUSH IMAGE   │
  │   (Push to registry)    │
  └────────────┬────────────┘
               ↓
  ┌─────────────────────────┐
  │   Stage 7: DEPLOY       │
  │   (Ship to production)  │
  └────────────┬────────────┘
               ↓
        ✅ Pipeline Complete
```

---

# 10. Post Actions
---

## Definition

**Post actions** are special blocks that execute after the pipeline or individual stages finish, regardless of — or conditional upon — the final build outcome. They are essential for **notifications**, **artifact archiving**, and **workspace cleanup**.

* **`success`** → fires only when the build completes without errors
* **`failure`** → fires exclusively when the build encounters a failing stage
* **`always`** → fires unconditionally after every build, pass or fail
* **`unstable`** → fires when the build succeeds but test results indicate instability
* **`cleanup`** → runs as the very last action, typically to wipe temporary files

---

# 11. Docker Integration
---

## Definition

Jenkins integrates **deeply and natively** with **Docker**, enabling pipelines to build Docker images directly from **Dockerfiles**, use Docker containers as **ephemeral build agents**, and push finished images to **container registries** seamlessly.

### Key Docker Capabilities in Jenkins

* **Build Images** → execute `docker build` within pipeline stages to create container images
* **Docker Agents** → run entire pipeline stages inside isolated Docker containers for environment consistency
* **Registry Push** → automatically push tagged images to Docker Hub, ECR, GCR, or private registries
* **Docker Compose** → orchestrate multi-container test environments during integration testing stages

```bash
# Building and pushing a Docker image within a Jenkins pipeline
docker build -t myapp:${BUILD_NUMBER} .
docker tag myapp:${BUILD_NUMBER} registry.io/myapp:latest
docker push registry.io/myapp:latest
```

---

# 12. GitHub Integration
---

## Definition

Jenkins connects securely to **GitHub** repositories using **webhooks** for automatic pipeline triggering and **credential-based authentication** (HTTPS tokens or SSH keys) for securely cloning private repositories.

* **Webhooks** → GitHub sends an HTTP POST to Jenkins whenever a push, PR, or tag event occurs, instantly triggering the pipeline
* **HTTPS Credentials** → Personal Access Tokens stored in Jenkins Credential Manager for authenticated repo access
* **SSH Keys** → public/private key pairs configured in Jenkins for secure, password-less repository operations
* **Branch Filtering** → pipelines can be configured to trigger only for specific branches or PR events

---

# 13. Build Triggering Mechanisms
---

## Definition

Jenkins supports multiple **build triggering strategies** that determine when and how a pipeline begins execution — from periodic polling to event-driven webhooks.

* **PollSCM** → Jenkins periodically checks the source repository at a cron-defined interval for new commits
* **Webhooks** → the repository proactively notifies Jenkins of changes in real-time (preferred and more efficient)
* **Pipeline Libraries** → shared libraries loaded at pipeline start that can contain reusable triggering logic
* **Manual Trigger** → developers can explicitly start a build from the Jenkins dashboard with optional parameters
* **Upstream Jobs** → a pipeline can be triggered automatically after another job completes successfully

```bash
# PollSCM cron expression — check for changes every 5 minutes
triggers {
    pollSCM('H/5 * * * *')
}
```

---

# 14. Maven Integration & Code Quality
---

## Definition

Jenkins integrates heavily with **Apache Maven** for Java project builds, leveraging **Global Tool Configuration** to manage Maven installations and plugins like **JaCoCo** and **Surefire** for code coverage and test reporting.

* **Global Tool Configuration** → centrally define Maven, JDK, and other tool installations that all pipelines can reference
* **JaCoCo Plugin** → generates detailed **code coverage reports** showing which lines and branches are exercised by tests
* **Surefire Reports** → parses Maven test results and presents them in Jenkins with pass/fail breakdowns per test class
* **Quality Gates** → combined with SonarQube, enforce minimum coverage and quality thresholds before allowing deployment

---

# 15. Backup & Restore
---

## Definition

Jenkins stores all critical data — job configurations, build histories, plugin settings, credentials, and user data — within the **Jenkins Home Directory** (`JENKINS_HOME`). Backing up this directory is the definitive method for disaster recovery.

* **What to Back Up** → the entire `JENKINS_HOME` folder, including `jobs/`, `plugins/`, `users/`, `secrets/`, and `config.xml`
* **Backup Tools** → ThinBackup plugin, periodic filesystem snapshots, or cloud storage sync
* **Restore Process** → replace the `JENKINS_HOME` directory with the backup copy and restart the Jenkins service

```bash
# Typical Jenkins Home path on Linux
/var/lib/jenkins/

# Backup command example
tar -czf jenkins-backup-$(date +%F).tar.gz /var/lib/jenkins/
```

---

# 16. End-to-End CI/CD Flow — The Complete Picture
---

## Definition

The **end-to-end Jenkins CI/CD flow** represents the complete automated journey of code from a developer's local machine all the way through to a production deployment — orchestrated entirely by Jenkins without any manual gates.

```
┌──────────────────────────────────────────────────────────────┐
│            COMPLETE JENKINS CI/CD LIFECYCLE                   │
└──────────────────────────────────────────────────────────────┘

  👨‍💻 Developer writes & commits code locally
                    ↓
  📤 Code is pushed to GitHub repository
                    ↓
  🔔 GitHub webhook fires → Jenkins pipeline auto-starts
                    ↓
  📥 CHECKOUT — Jenkins clones the latest source code
                    ↓
  🔨 BUILD — Application is compiled (mvn compile / npm build)
                    ↓
  🧪 TEST — Unit and integration tests are executed
                    ↓
  📦 PACKAGE — Artifacts are packaged (JAR / WAR / ZIP)
                    ↓
  🐳 DOCKER — Container image is built from Dockerfile
                    ↓
  🚀 PUSH — Image is pushed to container registry
                    ↓
  🌐 DEPLOY — Application is deployed to target environment
                    ↓
  ✅ NOTIFY — Team receives success/failure alerts via Slack
```

*"When every stage is automated and codified, the gap between writing code and shipping it to production shrinks to mere minutes."*

---

# 17. Key Summary
---

* **Jenkins** is an **open-source automation server** that fundamentally drives CI/CD pipelines for modern software teams
* It follows a **Master-Agent architecture** — the master orchestrates, agents execute
* Jenkins runs on **Windows, Linux, macOS, Docker, and Kubernetes**, making it universally deployable
* Over **1,800+ plugins** extend its capabilities to integrate with GitHub, Docker, Maven, AWS, Slack, SonarQube, and more
* **Security** is enforced through **authentication** (local, LDAP, OAuth) and **RBAC** for granular access control
* **Freestyle jobs** are GUI-configured and simple; **Pipeline jobs** are code-driven and scalable
* **Declarative pipelines** are structured and readable; **Scripted pipelines** offer raw Groovy flexibility
* The **Jenkinsfile** defines the entire CI/CD workflow as version-controlled code
* Standard stages flow: **Checkout → Build → Test → Package → Docker → Push → Deploy**
* **Post actions** handle notifications, artifact archiving, and workspace cleanup after every build
* Jenkins integrates **natively with Docker** for image building, container agents, and registry pushes
* **GitHub webhooks** enable real-time, event-driven pipeline triggering
* **Maven integration** with JaCoCo and Surefire delivers automated code coverage and test reporting
* **Backup** the `JENKINS_HOME` directory to ensure complete disaster recovery capability
