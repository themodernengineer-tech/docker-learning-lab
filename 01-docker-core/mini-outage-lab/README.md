# Docker Mini Outage Lab

A beginner-friendly Docker incident simulation designed to practice container lifecycle, port publishing, logs, inspection, troubleshooting, recovery, and container filesystem behavior.

---

# Scenario

A small Nginx web service is running inside a Docker container.

Users report that the website has suddenly become unavailable.

The objective is to:

1. deploy the service
2. verify that it works
3. inspect the running container
4. simulate an outage
5. diagnose the failure
6. recover the service
7. modify the running container
8. stop and restart it
9. completely destroy the container
10. recreate it and observe what happens to its data

---

# Architecture

```text
Browser
   |
   | http://localhost:8081
   v
Windows Host
   |
   | Port 8081
   v
Docker Port Mapping
   |
   | Container Port 80
   v
+-----------------------+
|     incident-web      |
|                       |
|        Nginx          |
|                       |
|    Port 80/tcp        |
+-----------------------+
```

---

# Step 1 — Deploy the Nginx Service

Run:

```powershell
docker run -d --name incident-web -p 8081:80 nginx
```

## Command Breakdown

`docker run`

Creates and starts a new container.

`-d`

Runs the container in detached mode.

`--name incident-web`

Assigns the container a human-readable name.

`-p 8081:80`

Maps:

```text
Host port 8081
      ↓
Container port 80
```

`nginx`

Uses the Nginx image.

---

# Step 2 — Verify the Service

Run:

```powershell
docker ps
```

Expected result:

The `incident-web` container should be running.

Open:

```text
http://localhost:8081
```

The Nginx welcome page should load.

## Evidence

### Container Running

![Service Running](screenshots/01-service-running.png)

### Website Working

![Website Working](screenshots/02-website-working.png)

---

# Step 3 — Inspect the Running Container

View logs:

```powershell
docker logs incident-web
```

Inspect metadata:

```powershell
docker inspect incident-web
```

Inspect running processes:

```powershell
docker top incident-web
```

Inspect resource usage:

```powershell
docker stats incident-web
```

Stop `docker stats` with:

```text
Ctrl + C
```

---

# Step 4 — Simulate an Outage

Stop the container:

```powershell
docker stop incident-web
```

Now try:

```powershell
docker ps
```

The container no longer appears because `docker ps` shows only running containers.

Check all containers:

```powershell
docker ps -a
```

The container should appear with a status similar to:

```text
Exited
```

## Evidence

![Container Exited](screenshots/03-container-exited.png)

---

# Step 5 — Diagnose the Incident

The first useful command was:

```powershell
docker ps
```

The service was missing from the running container list.

The next command was:

```powershell
docker ps -a
```

This revealed that the container still existed but had entered the `Exited` state.

## Root Cause

The container had been stopped.

The application was therefore unavailable because its main process was no longer running.

---

# Step 6 — Recover the Service

Restart the existing container:

```powershell
docker start incident-web
```

Verify:

```powershell
docker ps
```

Open again:

```text
http://localhost:8081
```

The service should now be available.

## Evidence

![Service Recovered](screenshots/04-service-recovered.png)

---

# Step 7 — Inspect Application Logs

Refresh the browser several times.

Then run:

```powershell
docker logs incident-web
```

Nginx should display HTTP request entries.

## Evidence

![Container Logs](screenshots/05-container-logs.png)

This demonstrates that Docker can capture application output written to standard output and standard error.

---

# Step 8 — Modify the Running Container

Enter the container:

```powershell
docker exec -it incident-web bash
```

If Bash is unavailable:

```powershell
docker exec -it incident-web sh
```

Navigate to the Nginx web directory:

```bash
cd /usr/share/nginx/html
```

List the files:

```bash
ls
```

Replace the homepage:

```bash
echo "<h1>Docker Incident Lab</h1><p>This page exists only inside this container.</p>" > index.html
```

Verify:

```bash
cat index.html
```

Exit:

```bash
exit
```

Open:

```text
http://localhost:8081
```

The customized page should now appear.

## Evidence

![Custom Page](screenshots/06-custom-page.png)

---

# Step 9 — Stop and Restart the Container

Stop:

```powershell
docker stop incident-web
```

Restart:

```powershell
docker start incident-web
```

Open:

```text
http://localhost:8081
```

The customized page should still exist.

## Observation

Stopping a container does not destroy its writable filesystem.

```text
Container
   ↓
Stopped
   ↓
Still Exists
   ↓
Writable State Preserved
```

---

# Step 10 — Destroy the Container

Stop the container:

```powershell
docker stop incident-web
```

Remove it:

```powershell
docker rm incident-web
```

Verify:

```powershell
docker ps -a
```

The `incident-web` container should no longer exist.

---

# Step 11 — Verify the Image Still Exists

Run:

```powershell
docker images
```

The Nginx image should still be available.

This demonstrates:

```text
Container removed
        ≠
Image removed
```

---

# Step 12 — Recreate the Container

Create a new container from the same image:

```powershell
docker run -d --name incident-web -p 8081:80 nginx
```

Open:

```text
http://localhost:8081
```

The original Nginx welcome page should be back.

The custom page is gone.

---

# Why Did the Custom Page Disappear?

The customized `index.html` existed in the writable layer of the original container.

When the container was removed:

```text
docker rm incident-web
```

that writable container state was destroyed.

The original Nginx image was unchanged.

Therefore:

```text
Nginx Image
     |
     +---- Old Container
     |       |
     |       +---- Custom index.html
     |
     |       docker rm
     |           ↓
     |       State deleted
     |
     +---- New Container
             |
             +---- Original Nginx files
```

---

# Key Docker Concepts Demonstrated

## Image vs Container

```text
Image
=
Reusable blueprint

Container
=
Runtime instance created from that blueprint
```

---

## Stop vs Remove

```text
docker stop
     ↓
Container stops
     ↓
Container still exists
     ↓
Writable state survives
```

while:

```text
docker rm
     ↓
Container removed
     ↓
Writable container state disappears
```

---

## `docker run` vs `docker start`

```text
docker run
=
Create a NEW container
```

```text
docker start
=
Start an EXISTING stopped container
```

---

## `docker exec`

```text
docker exec
=
Run an additional process inside an existing running container
```

---

## Port Publishing

```text
-p 8081:80
```

means:

```text
localhost:8081
      ↓
Docker Host
      ↓
Container port 80
      ↓
Nginx
```

---

# Troubleshooting Learned During the Lab

Two useful Docker errors occurred during setup.

## Container Name Conflict

Docker reported that:

```text
incident-web
```

was already in use.

Reason:

Container names must be unique, including stopped containers.

Useful commands:

```powershell
docker ps -a
docker rm -f incident-web
```

---

## Port Already Allocated

Docker reported:

```text
Bind for 0.0.0.0:8080 failed: port is already allocated
```

Reason:

Another process or container was already using host port `8080`.

Solution:

Use another host port:

```powershell
docker run -d --name incident-web -p 8081:80 nginx
```

This also demonstrated:

```text
Host ports must be unique
```

while the container can still listen on port `80`.

---

# Skills Practiced

- Docker images
- Docker containers
- container names
- detached mode
- port publishing
- container lifecycle
- `docker ps`
- `docker ps -a`
- `docker logs`
- `docker inspect`
- `docker top`
- `docker stats`
- `docker exec`
- `docker stop`
- `docker start`
- `docker rm`
- image vs container state
- basic incident diagnosis
- service recovery

---

# Main Lesson

The most important result from this lab was:

```text
Image
=
Reusable application blueprint

Container
=
Disposable runtime instance

Important data
=
Should not depend only on the container writable layer
```

This naturally leads to a later Docker topic:

**Volumes and persistent storage.**

---

# Status

- [x] Nginx container deployed
- [x] Host-to-container port mapping tested
- [x] Service verified in browser
- [x] Container logs inspected
- [x] Container metadata inspected
- [x] Outage simulated
- [x] Exited container diagnosed
- [x] Service recovered
- [x] Running container modified
- [x] Stop/restart persistence demonstrated
- [x] Container destruction demonstrated
- [x] Container-local state loss demonstrated

---

# Conclusion

This project demonstrated that troubleshooting Docker requires understanding container state rather than simply rerunning containers whenever something fails.

The incident was diagnosed using container status, logs, inspection, and lifecycle commands before recovery.

It also demonstrated why production applications should treat containers as disposable and keep important data outside the container's writable filesystem.