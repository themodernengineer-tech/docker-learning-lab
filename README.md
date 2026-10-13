# Docker Learning Lab

A hands-on Docker learning repository that starts from absolute beginner fundamentals and progresses step by step toward production deployment and modern industry practices.

The goal is not only to learn Docker commands.

The goal is to understand:

- what Docker is
- why containers are useful
- how images and containers work
- how to build Docker images
- how to persist data
- how containers communicate
- how to run multi-container applications
- how to secure and debug containers
- how to automate Docker workflows
- how to deploy containerized applications to production
- how modern Docker supply-chain and reliability practices work

---

# Learning Approach

Every topic follows the same practical learning loop:

```text
Learn the concept
      ↓
Understand the mental model
      ↓
Run commands
      ↓
Understand every important flag
      ↓
Complete a practical lab
      ↓
Run experiments
      ↓
Break something intentionally
      ↓
Debug and fix it
      ↓
Build a project or project milestone
      ↓
Document it on GitHub
      ↓
Create a LinkedIn post
      ↓
Create an Instagram Reel / YouTube Short
      ↓
Complete a topic checkpoint
      ↓
Move to the next topic
```

The learning path is designed to remain beginner-friendly while gradually introducing professional Docker practices.

---

# What This Repository Will Contain

```text
docker-learning-lab/
│
├── README.md
├── ROADMAP.md
│
├── 01-docker-core/
├── 02-dockerfiles-and-images/
├── 03-build-optimization/
├── 04-storage-and-volumes/
├── 05-docker-networking/
├── 06-docker-compose/
├── 07-config-and-secrets/
├── 08-registries-and-buildx/
├── 09-security/
├── 10-debugging-and-reliability/
├── 11-testing/
├── 12-cicd/
├── 13-production-docker/
├── 14-production-deployment/
├── 15-supply-chain-security/
├── 16-docker-operations/
├── 17-docker-internals/
├── 18-orchestration/
└── 19-capstone/
```

Folders will be created progressively as each topic is completed.

---

# Course Progression

The course follows this order:

```text
Docker Fundamentals
      ↓
Images and Containers
      ↓
Container Lifecycle
      ↓
Dockerfiles
      ↓
Build Optimization
      ↓
Volumes and Storage
      ↓
Networking
      ↓
Docker Compose
      ↓
Configuration and Secrets
      ↓
Registries and Multi-Architecture
      ↓
Security
      ↓
Debugging and Reliability
      ↓
Testing
      ↓
CI/CD
      ↓
Production Docker
      ↓
Production Deployment
      ↓
Supply-Chain Security
      ↓
Docker Operations
      ↓
Docker Internals
      ↓
Orchestration
      ↓
Final Capstone
```

---

# Current Progress

- [ ] 01. Docker Core — Fundamentals, Images and Containers
- [ ] 02. Dockerfiles and Custom Images
- [ ] 03. Build Optimization and Multi-Stage Builds
- [ ] 04. Storage, Volumes and Recovery
- [ ] 05. Docker Networking
- [ ] 06. Docker Compose
- [ ] 07. Configuration and Secrets
- [ ] 08. Registries, Buildx and Multi-Architecture
- [ ] 09. Docker Security
- [ ] 10. Debugging, Health and Reliability
- [ ] 11. Testing with Docker
- [ ] 12. CI/CD with GitHub Actions
- [ ] 13. Production Docker Architecture
- [ ] 14. Production Deployment and Recovery
- [ ] 15. Supply-Chain Security
- [ ] 16. Docker Host Operations
- [ ] 17. Docker Internals
- [ ] 18. Orchestration Fundamentals
- [ ] 19. Final Capstone Project

Current Topic:

```text
01 — Docker Core
```

---

# Practical Content After Every Topic

Every topic will include:

## 1. Practical Lab

A focused hands-on exercise.

Examples:

- running Ubuntu interactively
- serving a website with Nginx
- persisting PostgreSQL data
- connecting containers over a custom network

## 2. Experiment

A controlled experiment to understand Docker behavior.

Examples:

- stopping and restarting a container
- deleting a container while keeping a volume
- changing Dockerfile layer order
- breaking container DNS
- exhausting container memory

## 3. Break/Fix Exercise

A deliberately broken setup used for troubleshooting practice.

Examples:

- wrong port
- missing environment variable
- broken health check
- unavailable database
- volume permission error

## 4. Project or Project Milestone

Projects will be designed around real-world or emerging problems rather than generic demos whenever possible.

Examples include:

- disposable incident-response sandbox
- reproducible scientific computing environment
- privacy-preserving local AI gateway
- emergency information platform
- secure document-processing system
- container trust passport
- self-healing sensor platform
- disaster-recovery deployment
- local-first AI knowledge vault

## 5. GitHub Update

For each topic:

- files to create
- folder structure
- README notes
- architecture diagrams
- useful command outputs
- screenshots worth adding
- suggested commit message

## 6. LinkedIn Content

Each topic will include a complete LinkedIn post draft explaining:

- what was learned
- what was built
- key technical insight
- practical takeaway

## 7. Instagram Reel / YouTube Short

Each topic will include:

- reel topic
- hook
- 30–60 second script
- screen recording plan
- caption
- CTA
- hashtags where useful

---

# Final Goal

By the end of this repository, the learner should be able to take an application from source code to production:

```text
Source Code
    ↓
Dockerfile
    ↓
Optimized Image
    ↓
Security Checks
    ↓
Tests
    ↓
Docker Compose
    ↓
Container Registry
    ↓
CI/CD
    ↓
Production Server
    ↓
Reverse Proxy + HTTPS
    ↓
Application + Database
    ↓
Monitoring + Recovery
```

---

# Final Capstone

The final project will be a:

# Local-First AI Knowledge Vault

A privacy-oriented application designed around the growing need to use AI with sensitive personal or organizational data while keeping infrastructure under user control.

Target architecture:

```text
                    User
                     │
                     ▼
               Reverse Proxy
                     │
            ┌────────┴────────┐
            ▼                 ▼
        Frontend             API
                              │
                 ┌────────────┼────────────┐
                 ▼            ▼            ▼
              Worker       Database    Object Storage
                 │
                 ▼
           Local AI Service
```

The capstone will include:

- frontend
- backend API
- background worker
- database
- object storage
- local or mock AI service
- Dockerfiles
- multi-stage builds
- BuildKit
- Docker Compose
- private networks
- persistent volumes
- non-root containers
- health checks
- resource limits
- secrets
- automated testing
- CI/CD
- vulnerability scanning
- SBOM
- provenance
- multi-platform builds
- production deployment
- HTTPS
- backup and restore
- rollback strategy
- recovery validation

---

# References

The course will primarily follow current Docker documentation and use additional beginner-friendly references where useful.

Main references:

- Official Docker Documentation
- Docker Curriculum
- GeeksforGeeks Docker Tutorial

When older tutorials differ from current Docker behavior or recommendations, current Docker documentation will take priority.

---

# Learning Philosophy

The objective is not:

```text
memorize Docker commands
```

The objective is:

```text
understand containers
        ↓
build with confidence
        ↓
debug failures
        ↓
secure workloads
        ↓
automate workflows
        ↓
deploy safely
        ↓
recover when things break
```
