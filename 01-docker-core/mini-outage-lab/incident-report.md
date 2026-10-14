# Docker Mini Outage Lab — Incident Report

## Incident Summary

The scenario:
You are responsible for a small website running inside a Docker container. Users report that the site is unavailable. Your job is to determine what happened and recover the service.

The Nginx service running in the `incident-web` Docker container became unavailable on:

```text
http://localhost:8081
```

The objective was to investigate the failure and restore service using Docker container lifecycle and troubleshooting commands.

---

# Service Information

Container:

```text
incident-web
```

Image:

```text
nginx
```

Host Port:

```text
8081
```

Container Port:

```text
80
```

---

# Initial Verification

The service was initially deployed using:

```powershell
docker run -d --name incident-web -p 8081:80 nginx
```

Running containers were checked using:

```powershell
docker ps
```

The container was confirmed as running.

The Nginx website was successfully available at:

```text
http://localhost:8081
```

---

# Incident Simulation

The outage was intentionally created using:

```powershell
docker stop incident-web
```

After this command, the website became unavailable.

---

# Investigation

The first diagnostic command was:

```powershell
docker ps
```

The container was not listed.

This did not mean the container had been deleted.

The next command was:

```powershell
docker ps -a
```

This revealed that `incident-web` still existed but was in an exited state.

---

# Root Cause

The service was unavailable because the container had stopped.

The Nginx process was therefore no longer running.

The container itself had not been deleted.

---

# Recovery

The existing container was restarted using:

```powershell
docker start incident-web
```

Container state was verified using:

```powershell
docker ps
```

The service became available again at:

```text
http://localhost:8081
```

---

# Log Verification

Application logs were checked using:

```powershell
docker logs incident-web
```

Nginx HTTP requests were visible in the logs.

This confirmed that requests were reaching the application after recovery.

---

# Container State Experiment

The running container was modified using:

```powershell
docker exec -it incident-web bash
```

The Nginx homepage was replaced with:

```html
<h1>Docker Incident Lab</h1>
<p>This page exists only inside this container.</p>
```

After stopping and restarting the container, the custom page remained.

This demonstrated that:

```text
Stopping a container
does not remove
its writable filesystem
```

---

# Container Destruction Experiment

The container was then stopped and removed:

```powershell
docker stop incident-web
docker rm incident-web
```

A new container was created from the same Nginx image:

```powershell
docker run -d --name incident-web -p 8081:80 nginx
```

The customized webpage had disappeared.

The original Nginx page returned.

---

# Final Root Cause Analysis

The custom file existed only inside the writable layer of the original container.

Removing the container destroyed that writable state.

The Nginx image itself remained unchanged.

---

# Additional Issues Encountered

## Duplicate Container Name

During the exercise, Docker returned a container-name conflict.

Cause:

A previous container named:

```text
incident-web
```

already existed.

Resolution:

```powershell
docker rm -f incident-web
```

---

## Port Conflict

Docker also reported that host port `8080` was already allocated.

Instead of continuing to use port `8080`, the container was published using:

```powershell
-p 8081:80
```

This reinforced the difference between:

```text
Host Port
vs
Container Port
```

---

# Commands Used During Incident Response

```powershell
docker ps
docker ps -a
docker logs incident-web
docker inspect incident-web
docker top incident-web
docker stats incident-web
docker stop incident-web
docker start incident-web
docker exec -it incident-web bash
docker rm incident-web
docker images
```

---

# Lessons Learned

1. `docker ps` only displays running containers.
2. `docker ps -a` is useful when troubleshooting missing containers.
3. A stopped container still exists.
4. Restarting a stopped container preserves its writable state.
5. Removing a container deletes its writable state.
6. Removing a container does not remove its source image.
7. Docker logs are useful for investigating application activity.
8. Container names must be unique.
9. Host ports cannot be allocated to multiple listeners at the same time.
10. Important application data should not rely only on a container's writable filesystem.

---

# Incident Outcome

```text
Incident Detected
      ↓
Container Status Checked
      ↓
Exited Container Identified
      ↓
Container Restarted
      ↓
Service Restored
      ↓
Logs Verified
      ↓
Container State Tested
      ↓
Container Destroyed
      ↓
State Loss Confirmed
```

## Final Status

**Resolved**