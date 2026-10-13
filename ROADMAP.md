# Docker Learning Roadmap — Beginner to Production

This roadmap is fast-paced but beginner-friendly.

Each learning block covers several related concepts so progress is practical and efficient, while still preserving the correct learning order.

---

# Phase 1 — Beginner Foundations

## Topic 01 — Docker Core: Fundamentals, Images and Containers

### Learn

- What Docker is
- Why Docker exists
- Containers vs virtual machines
- Docker architecture
- Docker CLI
- Docker Engine
- Docker daemon
- Docker Desktop
- Docker Hub
- Registries
- Docker images
- Docker containers
- Image vs container
- Image tags
- Image digests
- Container IDs
- Container names
- Interactive containers
- Detached containers
- Foreground processes
- Container lifecycle
- Exit codes
- Port publishing
- Logs
- Container inspection

### Commands

```bash
docker --version
docker version
docker info

docker pull
docker images
docker image ls

docker run
docker run -it
docker run -d
docker run --name
docker run -p

docker ps
docker ps -a

docker start
docker stop
docker restart
docker kill

docker exec
docker logs
docker inspect
docker top
docker stats

docker rm
docker rmi
```

### Practical Labs

- Run `hello-world`
- Run Ubuntu interactively
- Create files inside a container
- Stop and restart the same container
- Run Nginx in detached mode
- Publish Nginx to a host port
- Inspect logs and processes

### Experiment

Create two containers from the same Ubuntu image and prove that they have independent writable filesystems.

### Break/Fix

- try accessing the wrong port
- stop the container and test again
- inspect logs and status
- restart it

### Project

## Disposable Incident Response Sandbox

Build a temporary Linux environment for inspecting logs, configuration files, or suspicious text artifacts without modifying the host system.

### GitHub

```text
01-docker-core/
├── README.md
├── commands.md
├── experiments.md
└── labs.md
```

### LinkedIn Topic

**What actually happens when you run a Docker container?**

### Reel Topic

**Docker Image vs Container in 60 seconds**

---

# Phase 2 — Building Docker Images

## Topic 02 — Dockerfiles and Custom Images

### Learn

- What a Dockerfile is
- Build context
- Dockerfile syntax
- Image layers
- Custom images
- Base images
- Container startup behavior

### Instructions

```dockerfile
FROM
WORKDIR
COPY
ADD
RUN
CMD
ENTRYPOINT
ENV
ARG
EXPOSE
USER
LABEL
HEALTHCHECK
```

### Important Comparisons

- `CMD` vs `ENTRYPOINT`
- `COPY` vs `ADD`
- build-time vs runtime configuration

### Commands

```bash
docker build
docker build -t
docker image history
docker inspect
docker run
```

### Lab

Containerize a small application.

### Experiment

Change one Dockerfile instruction and observe which build layers are reused.

### Project

## Reproducible Scientific Computing Capsule

Package a small Python data-processing workload so another user can reproduce the same result on another machine.

### LinkedIn

**I built my first Docker image — here is what a Dockerfile actually does**

### Reel

**FROM, COPY, RUN and CMD explained**

---

# Topic 03 — Build Optimization and Multi-Stage Builds

### Learn

- BuildKit
- layer caching
- cache invalidation
- `.dockerignore`
- dependency caching
- multi-stage builds
- builder images
- runtime images
- cache mounts
- secret mounts
- target stages
- image-size reduction

### Lab

Build:

```text
Unoptimized Image
        ↓
Optimized Image
        ↓
Multi-Stage Image
```

Compare:

- size
- build speed
- packages
- rebuild performance

### Project

## Carbon-Aware Container Optimization Lab

Optimize an intentionally bloated application and document improvements in:

- image size
- build duration
- transfer size
- startup time

### LinkedIn

**How I reduced a Docker image without changing the application**

### Reel

**Why multi-stage Docker builds matter**

---

# Phase 3 — Data and Communication

## Topic 04 — Storage, Volumes and Recovery

### Learn

- writable container layer
- ephemeral data
- named volumes
- anonymous volumes
- bind mounts
- tmpfs
- volume permissions
- volume lifecycle
- backup
- restore

### Commands

```bash
docker volume create
docker volume ls
docker volume inspect
docker volume rm
docker run -v
docker run --mount
```

### Lab

Run PostgreSQL with persistent storage.

Destroy the container and recreate it with the same volume.

### Experiment

Compare:

```text
Container filesystem
vs
Named volume
vs
Bind mount
```

### Project

## Personal Digital Vault Recovery Lab

Build a small service where the application and database containers can be destroyed and reconstructed from persistent data and backups.

### LinkedIn

**Why deleting a container should not delete your data**

### Reel

**Docker volumes explained with one experiment**

---

# Topic 05 — Docker Networking

### Learn

- container networking
- bridge network
- custom bridge network
- container IPs
- DNS
- service discovery
- network aliases
- port publishing
- localhost inside containers
- network isolation
- host networking
- none networking

### Commands

```bash
docker network ls
docker network create
docker network inspect
docker network connect
docker network disconnect
```

### Lab

Connect:

```text
API
 ↓
Database
```

without exposing the database publicly.

### Experiment

Try connecting containers:

- without a custom network
- with a custom network
- by IP
- by container name

### Project

## Privacy-Preserving Local AI Gateway

Architecture:

```text
Client
  ↓
Gateway
  ↓
Private AI Service
  ↓
Private Datastore
```

Only the gateway is publicly exposed.

### LinkedIn

**Why localhost behaves differently inside Docker**

### Reel

**How Docker containers find each other**

---

# Phase 4 — Multi-Container Applications

## Topic 06 — Docker Compose

### Learn

- Compose
- YAML
- services
- builds
- images
- ports
- volumes
- networks
- environment variables
- service dependencies
- restart policies
- health checks
- profiles
- Compose DNS

### Commands

```bash
docker compose up
docker compose up -d
docker compose down
docker compose ps
docker compose logs
docker compose exec
docker compose build
docker compose restart
docker compose config
```

### Lab

Convert a manually managed multi-container app into Compose.

### Project

## Community Emergency Information Hub

Build a multi-container system for sharing emergency information during:

- wildfires
- storms
- outages
- evacuation events

Architecture:

```text
Frontend
   ↓
API
   ↓
Database
   ↓
Volume
```

### LinkedIn

**How Docker Compose replaced a long list of docker run commands**

### Reel

**What docker compose up actually starts**

---

# Topic 07 — Configuration and Secrets

### Learn

- environment variables
- `.env`
- configuration precedence
- secrets
- Compose secrets
- BuildKit secrets
- development vs production configuration
- `.gitignore`
- runtime vs build-time configuration

### Lab

Run one application in:

```text
Development
Testing
Production
```

using different configuration.

### Experiment

Intentionally leak a secret into an image layer, inspect it, then fix the design.

### Project

## Secure Configuration Lab

Refactor a containerized application so:

- secrets are not committed
- secrets are not baked into images
- environment-specific config remains portable

### LinkedIn

**Environment variables are not the same as secrets**

### Reel

**One Docker secret mistake beginners make**

---

# Phase 5 — Image Distribution and Portability

## Topic 08 — Registries, Docker Hub, Buildx and Multi-Architecture

### Learn

- Docker registries
- Docker Hub
- repositories
- tags
- semantic versioning
- digests
- authentication
- push/pull
- private registries
- Buildx
- AMD64
- ARM64
- image manifests
- multi-platform publishing

### Commands

```bash
docker login
docker tag
docker push
docker pull

docker buildx ls
docker buildx create
docker buildx build
docker buildx imagetools inspect
```

### Lab

Push an application image to Docker Hub.

### Experiment

Compare:

```text
latest
vs
version tag
vs
commit tag
vs
digest
```

### Project

## Universal Edge Application

Publish an image that can run on:

- AMD64 cloud servers
- ARM64 edge devices
- Apple Silicon systems

### LinkedIn

**How one Docker image can support multiple CPU architectures**

### Reel

**AMD64 vs ARM64 Docker images**

---

# Phase 6 — Security and Reliability

## Topic 09 — Docker Security

### Learn

- root vs non-root
- least privilege
- minimal base images
- trusted base images
- capabilities
- privileged containers
- read-only filesystems
- resource limits
- Docker socket risks
- secrets
- vulnerability scanning

### Lab

Start with an insecure container and harden it.

### Project

## Secure Multi-Tenant Document Processor

Build a containerized file-processing system with:

- non-root execution
- private network
- temporary processing storage
- restricted permissions
- resource limits

### LinkedIn

**Docker containers are not automatically secure**

### Reel

**3 Docker security mistakes beginners make**

---

# Topic 10 — Debugging, Health and Reliability

### Learn

- logs
- inspect
- exit codes
- health checks
- restart policies
- CPU limits
- memory limits
- OOM behavior
- graceful shutdown
- SIGTERM
- PID 1
- Docker events

### Commands

```bash
docker logs
docker inspect
docker stats
docker top
docker events
```

### Break/Fix Lab

Diagnose:

- wrong port
- broken DNS
- failed health check
- database unavailable
- missing variable
- memory exhaustion

### Project

## Self-Healing Sensor Processing Platform

Simulate workers processing environmental sensor data and recover from:

- service crashes
- slow dependencies
- failed workers
- high memory usage

### LinkedIn

**A running container is not necessarily a healthy container**

### Reel

**Docker health checks in 60 seconds**

---

# Phase 7 — Testing and Automation

## Topic 11 — Testing with Docker

### Learn

- isolated test environments
- disposable databases
- integration testing
- deterministic test data
- Compose testing stacks
- smoke tests
- cleanup

### Project

## Disposable Pull-Request Environment

Create temporary:

```text
Application
Database
Cache
Test Runner
```

Run tests and destroy everything afterward.

### LinkedIn

**Why containers are useful for integration testing**

### Reel

**Spin up a disposable test database with Docker**

---

# Topic 12 — CI/CD with GitHub Actions

### Learn

- CI/CD
- GitHub Actions
- Docker builds in CI
- caching
- registry authentication
- image tagging
- secrets
- automated testing
- automated publishing
- vulnerability gates

### Pipeline

```text
Git Push
   ↓
Test
   ↓
Build
   ↓
Scan
   ↓
Push
```

### Project

## Secure Container Factory

Create a pipeline that:

- tests code
- builds image
- scans image
- tags image
- publishes image only when checks pass

### LinkedIn

**From git push to Docker Hub automatically**

### Reel

**Git push → Docker image**

---

# Phase 8 — Production

## Topic 13 — Production Docker Architecture

### Learn

- production Dockerfiles
- immutable containers
- reverse proxy
- Nginx
- TLS
- HTTPS
- domains
- production Compose
- persistent data
- logging
- resource controls
- versioning
- rollback concepts

### Project

## Digital Legacy Vault

Create a production-style application architecture for preserving important records.

Include:

- reverse proxy
- API
- database
- storage
- health checks
- versioned images

### LinkedIn

**What changes when Docker moves from development to production**

### Reel

**Development Docker vs Production Docker**

---

# Topic 14 — Production Deployment and Recovery

### Learn

- Linux server setup
- SSH
- Docker Engine
- firewall basics
- DNS
- HTTPS
- production Compose
- registry authentication
- deployment
- rollback
- backup
- restore
- deployment validation
- disaster recovery concepts

### Project

## Disaster Recovery Deployment Drill

Process:

```text
Deploy
 ↓
Create data
 ↓
Backup
 ↓
Destroy deployment
 ↓
Restore
 ↓
Verify
 ↓
Deploy broken release
 ↓
Rollback
```

### LinkedIn

**A deployment is not production-ready until recovery is tested**

### Reel

**How to prove your Docker backup actually works**

---

# Phase 9 — Advanced Industry Practices

## Topic 15 — Supply-Chain Security

### Learn

- vulnerabilities
- CVEs
- Docker Scout
- SBOM
- SPDX
- CycloneDX
- provenance
- attestations
- image digests
- policy checks
- signed artifact concepts

### Commands

```bash
docker scout quickview
docker scout cves
docker scout sbom
docker scout policy
```

### Project

## Container Trust Passport

Generate for each image:

```text
Image
+
SBOM
+
Provenance
+
Vulnerability Report
+
Policy Result
```

### LinkedIn

**How do you know what is really inside a container image?**

### Reel

**SBOM explained in 60 seconds**

---

# Topic 16 — Docker Host Operations

### Learn

- disk usage
- stale images
- dangling resources
- build cache
- log growth
- pruning
- Docker contexts
- remote Docker hosts
- daemon configuration concepts

### Commands

```bash
docker system df
docker system prune
docker image prune
docker container prune
docker volume prune
docker network prune
docker builder prune
docker context ls
```

### Project

## Docker Host Capacity Guardian

Build a simple operational report that identifies:

- unused images
- unused volumes
- high disk usage
- resource-heavy containers
- stale resources

### LinkedIn

**Why Docker hosts eventually run out of disk**

### Reel

**The Docker cleanup commands every developer should know**

---

# Phase 10 — Advanced Understanding

## Topic 17 — Docker Internals

### Learn

- Docker Engine
- Docker API
- dockerd
- containerd
- OCI
- runc concepts
- namespaces
- cgroups
- OverlayFS
- process isolation
- filesystem layers
- network namespaces

### Project

## Container Without the Magic

Inspect a container from the host perspective:

- process
- namespaces
- filesystem
- networking
- resource controls

### LinkedIn

**Containers are processes, not tiny virtual machines**

### Reel

**What Docker really creates when a container starts**

---

# Topic 18 — Orchestration Fundamentals

### Learn

- why orchestration exists
- desired state
- nodes
- managers
- workers
- services
- replicas
- scaling
- service discovery
- overlay networks
- rolling updates
- secrets
- failure recovery
- Docker Swarm concepts

### Project

## Distributed Crisis Notification System

Deploy replicated services and simulate:

- node failure
- scaling
- service restart
- rolling updates

### LinkedIn

**When one Docker host is no longer enough**

### Reel

**Containers vs container orchestration**

---

# Phase 11 — Final Capstone

## Topic 19 — Local-First AI Knowledge Vault

Build a privacy-oriented AI platform that keeps sensitive data within user-controlled infrastructure.

### Architecture

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

### Required Docker Features

- Dockerfiles
- `.dockerignore`
- BuildKit
- multi-stage builds
- custom networks
- persistent volumes
- Docker Compose
- health checks
- restart policies
- environment configuration
- secrets
- non-root users
- resource limits
- testing
- CI/CD
- Docker Hub
- versioned images
- multi-platform publishing
- vulnerability scanning
- SBOM
- provenance
- production deployment
- reverse proxy
- HTTPS
- backup
- restore
- rollback
- failure recovery

### Final Pipeline

```text
Source Code
    ↓
Tests
    ↓
Docker Build
    ↓
Security Scan
    ↓
SBOM + Provenance
    ↓
Registry
    ↓
Production Deployment
    ↓
Smoke Test
    ↓
Monitoring
    ↓
Recovery Plan
```

---

# Topic Completion Template

Every topic must end with:

```text
[ ] Concept understood
[ ] Commands practiced
[ ] Lab completed
[ ] Experiment completed
[ ] Break/fix exercise completed
[ ] Project milestone completed
[ ] GitHub updated
[ ] LinkedIn post prepared
[ ] Reel script prepared
[ ] Topic checkpoint passed
```

---

# Current Topic

```text
01 — Docker Core: Fundamentals, Images and Containers
```
