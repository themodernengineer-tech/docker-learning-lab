# Docker Core — Practical Labs

This document contains the hands-on labs completed while learning Docker fundamentals, images, containers, processes, ports, logs, and the container lifecycle.

---

# Lab 1 — Run the First Docker Container

## Objective

Verify that Docker is working and understand what happens when Docker runs an image for the first time.

## Command

```bash
docker run hello-world
```

## What the Command Means

```text
docker run hello-world
│      │       │
│      │       └── Docker image
│      │
│      └── Create and start a new container
│
└── Docker CLI
```

## What Happens Internally

```text
Docker CLI
    ↓
Docker Engine
    ↓
Check for hello-world image locally
    ↓
Image missing?
    ↓
Pull image from Docker Hub
    ↓
Create container
    ↓
Start container
    ↓
Execute application
    ↓
Application prints message
    ↓
Application exits
    ↓
Container stops
```

## Key Observation

The `hello-world` container does not continue running because the process inside it finishes.

A container normally remains running only while its main process remains running.

---

# Lab 2 — Inspect Running and Stopped Containers

## Show Running Containers

```bash
docker ps
```

This displays containers that are currently running.

Because the `hello-world` container already finished, it may not appear here.

## Show All Containers

```bash
docker ps -a
```

The `-a` option means:

```text
all
```

This includes:

- running containers
- stopped containers
- exited containers

## Key Observation

```text
Stopped Container
       ≠
Deleted Container
```

A container can stop while still existing on the Docker host.

---

# Lab 3 — Interactive Ubuntu Container

## Objective

Start an Ubuntu container and interact directly with the shell running inside it.

## Command

```bash
docker run -it ubuntu bash
```

## Command Breakdown

```text
docker
└── Docker CLI

run
└── Create and start a new container

-i
└── Keep standard input open

-t
└── Allocate an interactive terminal

ubuntu
└── Image used to create the container

bash
└── Process started inside the container
```

The combined option:

```bash
-it
```

allows us to interact with Bash inside the container.

---

# Lab 4 — Explore the Ubuntu Container

After entering the container, the terminal looked similar to:

```text
root@74b2542dd509:/#
```

This means:

```text
root
│
└── Current user

74b2542dd509
│
└── Container hostname

/
│
└── Current directory

#
└── Root shell prompt
```

## Check Current Directory

```bash
pwd
```

Expected:

```text
/
```

## Check Current User

```bash
whoami
```

Expected:

```text
root
```

## Check Container Hostname

```bash
hostname
```

The hostname usually resembles the container ID.

Example:

```text
74b2542dd509
```

## Check Operating System

```bash
cat /etc/os-release
```

This confirms that the environment inside the container is Ubuntu.

## Check Processes

```bash
ps
```

This displays processes running inside the container.

---

# Lab 5 — Create Data Inside the Container

Create a file:

```bash
touch /root/docker-test.txt
```

Verify it:

```bash
ls /root
```

Expected:

```text
docker-test.txt
```

The file exists inside this container's writable filesystem.

---

# Lab 6 — Stop the Container by Exiting Bash

Run:

```bash
exit
```

This returns to the host PowerShell terminal.

Why did the container stop?

The main process inside this container was:

```text
bash
```

The lifecycle was:

```text
Container starts
      ↓
Bash starts
      ↓
Bash becomes main process
      ↓
User runs exit
      ↓
Bash terminates
      ↓
Container stops
```

## Verify

```bash
docker ps
```

The Ubuntu container should no longer appear.

Now:

```bash
docker ps -a
```

The Ubuntu container should appear with a status similar to:

```text
Exited
```

---

# Lab 7 — Restart the Same Container

Find the container ID:

```bash
docker ps -a
```

Then restart it and attach the terminal:

```bash
docker start -ai <container-id>
```

Example:

```bash
docker start -ai 74b2542dd509
```

## Options

```text
-a
└── Attach terminal output

-i
└── Keep input interactive
```

Now verify whether the previously created file still exists:

```bash
ls /root
```

Expected:

```text
docker-test.txt
```

## Key Observation

Stopping a container does **not** remove its writable filesystem.

```text
Container created
      ↓
File created
      ↓
Container stopped
      ↓
Container restarted
      ↓
File still exists
```

However, this should not be confused with permanent application data storage.

Later we will learn Docker volumes for persistent data.

---

# Lab 8 — Run Nginx in Detached Mode

Exit the Ubuntu container:

```bash
exit
```

Now run Nginx:

```bash
docker run -d -p 8080:80 --name nginx-lab nginx
```

## Command Breakdown

### `docker run`

Create and start a new container.

### `-d`

Detached mode.

The container runs in the background instead of occupying the terminal.

### `-p 8080:80`

Publish a container port.

```text
8080 : 80
  │     │
  │     └── Port inside container
  │
  └── Port on host machine
```

Traffic flow:

```text
Browser
   ↓
localhost:8080
   ↓
Windows Host
   ↓
Docker port mapping
   ↓
Container port 80
   ↓
Nginx
```

### `--name nginx-lab`

Assign a human-readable name to the container.

Instead of using:

```text
74b2542dd509
```

we can use:

```text
nginx-lab
```

### `nginx`

The image used to create the container.

---

# Lab 9 — Verify Nginx Is Running

Run:

```bash
docker ps
```

The `nginx-lab` container should appear as running.

Open:

```text
http://localhost:8080
```

The Nginx welcome page should appear.

## Key Observation

Nginx listens on:

```text
Container Port 80
```

but we access it using:

```text
Host Port 8080
```

because of:

```bash
-p 8080:80
```

---

# Lab 10 — Inspect Container Logs

Run:

```bash
docker logs nginx-lab
```

Refresh the browser several times.

Then run again:

```bash
docker logs nginx-lab
```

Additional HTTP requests should appear.

## Key Observation

Applications running inside containers generally write logs to:

```text
stdout
stderr
```

Docker captures this output and exposes it using:

```bash
docker logs
```

---

# Lab 11 — Inspect Container Metadata

Run:

```bash
docker inspect nginx-lab
```

This returns detailed JSON describing the container.

Information includes:

- container ID
- container name
- image
- state
- environment variables
- networking
- IP address
- ports
- mounts
- process configuration

`docker inspect` becomes an important debugging command later in the course.

---

# Lab 12 — Execute Commands Inside a Running Container

Run:

```bash
docker exec -it nginx-lab bash
```

If Bash is unavailable:

```bash
docker exec -it nginx-lab sh
```

## Important Difference

Earlier we used:

```bash
docker run
```

Now we use:

```bash
docker exec
```

They are different.

```text
docker run
    ↓
Creates a NEW container
```

while:

```text
docker exec
    ↓
Runs another process inside an EXISTING running container
```

Inside the Nginx container, run:

```bash
hostname
pwd
ls
ps
```

Exit:

```bash
exit
```

The Nginx container should continue running.

Why?

Because exiting this shell did not terminate the main Nginx process.

---

# Lab 13 — Stop and Restart Nginx

Stop:

```bash
docker stop nginx-lab
```

Check:

```bash
docker ps
```

The container should disappear.

Check all containers:

```bash
docker ps -a
```

The container should still exist.

Restart:

```bash
docker start nginx-lab
```

Check:

```bash
docker ps
```

Visit:

```text
http://localhost:8080
```

Nginx should work again.

---

# Lab 14 — Remove the Nginx Container

Stop it:

```bash
docker stop nginx-lab
```

Remove it:

```bash
docker rm nginx-lab
```

Verify:

```bash
docker ps -a
```

The `nginx-lab` container should no longer exist.

---

# Lab 15 — Container vs Image Lifecycle

Check available images:

```bash
docker images
```

The Nginx image should still exist even though the Nginx container was deleted.

This demonstrates:

```text
Docker Image
     │
     ├── Container 1
     ├── Container 2
     └── Container 3
```

Containers are created **from** images.

Removing a container does not automatically remove its source image.

---

# Lab 16 — Two Containers From the Same Image

This experiment demonstrates that containers created from the same image have independent writable filesystems.

Create the first container:

```bash
docker run -it --name ubuntu-one ubuntu bash
```

Inside:

```bash
touch /root/only-in-one.txt
ls /root
```

Expected:

```text
only-in-one.txt
```

Exit:

```bash
exit
```

Create another container from the exact same Ubuntu image:

```bash
docker run -it --name ubuntu-two ubuntu bash
```

Inside:

```bash
ls /root
```

The file:

```text
only-in-one.txt
```

should **not** exist.

Exit:

```bash
exit
```

## Why?

Both containers share the same underlying Ubuntu image.

But each container receives its own writable container layer.

```text
                    Ubuntu Image
                         │
                ┌────────┴────────┐
                │                 │
                ▼                 ▼
          ubuntu-one          ubuntu-two
                │                 │
                ▼                 ▼
        Writable Layer A   Writable Layer B
                │                 │
     only-in-one.txt          no file
```

## Key Concept

```text
Same Image
≠
Same Container State
```

---

# Lab 17 — Inspect Both Containers

Run:

```bash
docker ps -a
```

You should see:

```text
ubuntu-one
ubuntu-two
```

Both were created from:

```text
ubuntu
```

but they are separate container instances.

Inspect one:

```bash
docker inspect ubuntu-one
```

Inspect the other:

```bash
docker inspect ubuntu-two
```

Notice that they have different:

- container IDs
- hostnames
- writable states
- metadata

---

# Lab 18 — Cleanup

List containers:

```bash
docker ps -a
```

Remove the Ubuntu test containers:

```bash
docker rm ubuntu-one ubuntu-two
```

If a container is still running, stop it first:

```bash
docker stop ubuntu-one ubuntu-two
```

and then remove:

```bash
docker rm ubuntu-one ubuntu-two
```

Check:

```bash
docker ps -a
```

---

# Main Concepts Learned

## Image

An image is a packaged, read-only template used to create containers.

```text
Image
  ↓
Blueprint
```

## Container

A container is an instance created from an image.

```text
Image
  ↓
Container
```

One image can create many independent containers.

```text
             Image
               │
       ┌───────┼───────┐
       ▼       ▼       ▼
Container A Container B Container C
```

## Interactive Mode

```bash
-it
```

allows interaction with a process inside a container.

## Detached Mode

```bash
-d
```

runs the container in the background.

## Port Publishing

```bash
-p 8080:80
```

maps:

```text
Host Port 8080
       ↓
Container Port 80
```

## `docker run`

Creates a new container.

## `docker start`

Starts an existing stopped container.

## `docker exec`

Runs an additional command inside an already-running container.

## `docker stop`

Stops the main container process gracefully.

## `docker rm`

Deletes a container.

## `docker rmi`

Deletes an image.

---

# Important Mental Model

```text
Docker Registry
      ↓
Docker Image
      ↓
docker run
      ↓
Container
      ↓
Main Process
      ↓
Application runs
```

Container lifetime generally follows its main process:

```text
Main Process Running
        ↓
Container Running

Main Process Stops
        ↓
Container Stops
```

---

# Commands Practiced

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
docker run -p
docker run --name

docker ps
docker ps -a

docker start
docker stop
docker restart

docker exec
docker logs
docker inspect
docker top
docker stats

docker rm
docker rmi
```

---

# Key Takeaways

- Docker images and containers are different objects.
- Images are templates used to create containers.
- Multiple containers can be created from the same image.
- Every container gets its own writable filesystem layer.
- Stopping a container does not delete it.
- Restarting a stopped container preserves its writable state.
- Removing a container does not automatically remove its image.
- A container normally runs while its main process remains alive.
- `docker run` creates a new container.
- `docker start` restarts an existing container.
- `docker exec` starts an additional process inside a running container.
- Port publishing connects a host port to a container port.
- Docker logs can be used to inspect application output.

---

# Topic 01 Lab Checklist

- [x] Docker installation verified
- [x] `hello-world` container executed
- [x] Ubuntu interactive container started
- [x] Container filesystem explored
- [x] File created inside a container
- [x] Container stopped and restarted
- [x] Container filesystem persistence observed
- [ ] Nginx container started
- [ ] Port `8080:80` tested
- [ ] Nginx logs inspected
- [ ] `docker inspect` practiced
- [ ] `docker exec` practiced
- [ ] Nginx stopped and restarted
- [ ] Container removed
- [ ] Two-container filesystem experiment completed
- [ ] Test containers cleaned up

---

# Final Observation

The most important lesson from these labs is:

```text
Image = reusable blueprint

Container = isolated runtime instance of that blueprint
```

Understanding this distinction is the foundation for everything that follows in Docker.
