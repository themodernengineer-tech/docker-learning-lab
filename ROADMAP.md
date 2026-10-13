# Docker Learning Roadmap

This roadmap tracks the progression from Docker beginner fundamentals to production deployment.

---

## Phase 1 — Foundations

### 01. Docker Fundamentals

Topics:

- What Docker is
- Why containers exist
- Containers vs virtual machines
- Docker architecture
- Docker Engine
- Docker CLI
- Docker daemon
- Docker Desktop
- Docker registries
- Docker Hub
- Installing Docker
- Verifying Docker installation
- Running the first container

Lab:

- Run `hello-world`
- Inspect Docker installation
- Understand what happens when a container starts

Status:

- [ ] Not Started

---

### 02. Images and Containers

Topics:

- Docker images
- Docker containers
- Image vs container
- Image layers
- Container lifecycle
- Pulling images
- Running containers
- Naming containers
- Detached mode
- Interactive mode
- Port publishing

Lab:

- Run an Nginx web server
- Inspect containers
- Stop and remove containers

Status:

- [ ] Not Started

---

### 03. Container Lifecycle and Interaction

Topics:

- `docker run`
- `docker start`
- `docker stop`
- `docker restart`
- `docker exec`
- `docker logs`
- `docker inspect`
- `docker cp`
- `docker stats`
- Container processes
- Foreground vs background containers

Lab:

- Run an Ubuntu container
- Execute commands inside the container
- Inspect processes and files

Status:

- [ ] Not Started

---

## Phase 2 — Building Images

### 04. Dockerfiles

Topics:

- Dockerfile fundamentals
- `FROM`
- `WORKDIR`
- `COPY`
- `ADD`
- `RUN`
- `CMD`
- `ENTRYPOINT`
- `ENV`
- `ARG`
- `EXPOSE`
- `USER`
- `HEALTHCHECK`
- Building images

Lab:

- Build a custom application image

Status:

- [ ] Not Started

---

### 05. Image Optimization

Topics:

- Docker layers
- Build cache
- Build context
- `.dockerignore`
- Cache invalidation
- Base image selection
- Reducing image size
- BuildKit fundamentals

Lab:

- Compare unoptimized and optimized Docker images

Status:

- [ ] Not Started

---

## Phase 3 — Data and Networking

### 06. Volumes and Storage

Topics:

- Container writable layer
- Persistent storage
- Named volumes
- Anonymous volumes
- Bind mounts
- Temporary storage
- Volume lifecycle

Lab:

- Create a database container with persistent storage

Status:

- [ ] Not Started

---

### 07. Docker Networking

Topics:

- Container networking
- Bridge networks
- Custom networks
- Port publishing
- Container DNS
- Service discovery
- Container-to-container communication
- Host networking
- Network isolation

Lab:

- Connect an application container to a database container

Status:

- [ ] Not Started

---

## Phase 4 — Multi-Container Applications

### 08. Docker Compose

Topics:

- `compose.yaml`
- Services
- Images
- Builds
- Ports
- Networks
- Volumes
- Environment variables
- Dependencies
- Health checks
- Restart policies

Lab:

- Build a multi-container application stack

Status:

- [ ] Not Started

---

### 09. Configuration and Secrets

Topics:

- Environment variables
- `.env`
- Compose interpolation
- Development configuration
- Production configuration
- Secrets
- Git ignore strategy
- Build-time vs runtime configuration

Lab:

- Run one application with multiple environment configurations

Status:

- [ ] Not Started

---

## Phase 5 — Registries and Builds

### 10. Docker Registries

Topics:

- Docker Hub
- Registries
- Repositories
- Tags
- Image versions
- Image digests
- Authentication
- Pushing images
- Pulling images

Lab:

- Publish a Docker image to a registry

Status:

- [ ] Not Started

---

### 11. Multi-Stage Builds

Topics:

- Builder stages
- Runtime stages
- Copying artifacts between stages
- Production image optimization
- Removing unnecessary build dependencies

Lab:

- Convert an application into a multi-stage build

Status:

- [ ] Not Started

---

### 12. Buildx and Multi-Architecture

Topics:

- BuildKit
- Buildx
- AMD64
- ARM64
- Multi-platform builds
- Image manifests

Lab:

- Build one image for AMD64 and ARM64

Status:

- [ ] Not Started

---

## Phase 6 — Security and Operations

### 13. Docker Security

Topics:

- Running as non-root
- Least privilege
- Linux capabilities
- Read-only filesystems
- Secrets
- Vulnerability scanning
- Trusted base images
- Resource limits
- Docker socket risks
- Supply-chain security

Lab:

- Harden an intentionally insecure container

Status:

- [ ] Not Started

---

### 14. Debugging and Troubleshooting

Topics:

- Container logs
- Inspection
- Processes
- Networking failures
- Volume issues
- Permission errors
- Port conflicts
- Environment problems
- Container exit codes

Lab:

- Diagnose deliberately broken Docker environments

Status:

- [ ] Not Started

---

### 15. Observability and Reliability

Topics:

- Logs
- `stdout`
- `stderr`
- Logging drivers
- Health checks
- Restart policies
- CPU limits
- Memory limits
- Graceful shutdown
- Container health states

Lab:

- Add health checks and resource controls to an application

Status:

- [ ] Not Started

---

## Phase 7 — Testing and Automation

### 16. Testing with Docker

Topics:

- Disposable test environments
- Integration tests
- Test databases
- Compose-based testing
- Reproducible test environments
- Cleanup strategies

Lab:

- Run automated integration tests using containers

Status:

- [ ] Not Started

---

### 17. GitHub Actions and CI/CD

Topics:

- CI/CD fundamentals
- GitHub Actions
- Workflow YAML
- Automated tests
- Automated Docker builds
- Image tagging
- Registry authentication
- Security scanning
- Build caching

Lab:

```text
Git Push
   ↓
GitHub Actions
   ↓
Test
   ↓
Build
   ↓
Scan
   ↓
Push Image
```

Status:

- [ ] Not Started

---

## Phase 8 — Production

### 18. Production Docker

Topics:

- Production Dockerfiles
- Minimal images
- Non-root containers
- Immutable infrastructure
- Health checks
- Resource controls
- Logging
- Secrets
- Persistent data
- Reverse proxies
- Deployment strategies
- Rollbacks

Lab:

- Prepare the application stack for production

Status:

- [ ] Not Started

---

### 19. Production Deployment

Topics:

- Linux server setup
- SSH
- Docker Engine installation
- Firewalls
- Production Compose
- Container registries
- Domains
- Reverse proxies
- HTTPS/TLS
- Deployment updates
- Rollbacks
- Backups

Lab:

```text
Development
    ↓
GitHub
    ↓
CI/CD
    ↓
Container Registry
    ↓
Production Server
    ↓
Docker
    ↓
Application
```

Status:

- [ ] Not Started

---

## Phase 9 — Advanced Docker

### 20. Docker Swarm

Topics:

- Container orchestration
- Nodes
- Managers
- Workers
- Services
- Replicas
- Scaling
- Rolling updates
- Overlay networks
- Secrets

Lab:

- Create a small Docker Swarm cluster

Status:

- [ ] Not Started

---

### 21. Docker Internals

Topics:

- Docker Engine internals
- Docker API
- `containerd`
- OCI
- Linux namespaces
- Control groups
- OverlayFS
- Network namespaces
- Container isolation

Lab:

- Inspect how Docker uses Linux primitives

Status:

- [ ] Not Started

---

## Phase 10 — Capstone

### 22. Production Capstone

The final application will combine:

- Dockerfiles
- Multi-stage builds
- Frontend
- Backend
- Database
- Persistent storage
- Docker networking
- Docker Compose
- Configuration
- Secrets
- Health checks
- Security
- Testing
- GitHub Actions
- Container registry
- Production deployment
- HTTPS
- Logging
- Backup strategy
- Rollback strategy

Target architecture:

```text
                   Internet
                      │
                      ▼
                Reverse Proxy
                      │
            ┌─────────┴─────────┐
            ▼                   ▼
        Frontend             Backend
                                │
                      ┌─────────┴─────────┐
                      ▼                   ▼
                   Database             Cache
                      │
                      ▼
               Persistent Storage
```

Status:

- [ ] Not Started

---

# Progress

```text
Topics Completed: 0 / 22
Current Topic: 01 — Docker Fundamentals
```

The roadmap will be updated after each completed topic.
