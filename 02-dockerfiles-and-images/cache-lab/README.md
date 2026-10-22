# Docker Image Layers and Build Cache Lab

## Overview

This lab explores how Docker builds images using layers and how the Docker build cache can reuse previous build results.

The objective was not simply to build a Python container. The application was intentionally kept small so the experiment could focus on Docker's build behavior.

The main question investigated in this lab was:

> Why can changing one application file cause Docker to repeat dependency installation, and how can Dockerfile structure prevent that unnecessary work?

---

## Concepts Explored

A Docker image is not created as one large filesystem snapshot. Docker builds images from a sequence of instructions and stores filesystem changes as reusable layers.

When an image is rebuilt, Docker evaluates each build step and determines whether an existing result can be reused.

If nothing affecting a step has changed, Docker can reuse its cached result.

If an instruction or one of its inputs changes, that step must be rebuilt and later dependent steps may also need to be reevaluated.

This behavior makes Dockerfile instruction ordering important.

Files that change infrequently, such as dependency manifests, should generally be handled before frequently changing application source files.

---

## Project Structure

```text
cache-lab/
├── app.py
├── requirements.txt
├── Dockerfile
└── README.md
```

---

## Application

The application uses the Python `requests` package and displays the installed package version.

`app.py`:

```python
import requests

print("Docker Cache Lab")
print("Requests version:", requests.__version__)
```

The application itself is intentionally simple. Its purpose is to provide a dependency and an application file that can be changed independently during the Docker build-cache experiments.

---

## Dependency File

`requirements.txt`:

```text
requests==2.32.5
```

Pinning the dependency version makes the build more predictable and gives Docker a stable dependency definition to work with.

---

## Experiment 1 — Intentionally Inefficient Dockerfile

The first Dockerfile used a broad `COPY` instruction before installing dependencies.

```dockerfile
FROM python:3.13-slim

WORKDIR /app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt

CMD ["python", "app.py"]
```

The important sequence was:

```text
COPY everything
      ↓
Install dependencies
```

The image was built using:

```bash
docker build -t cache-lab:v1 .
```

The container was then executed using:

```bash
docker run --rm cache-lab:v1
```

Expected application output:

```text
Docker Cache Lab
Requests version: 2.32.5
```

---

## Build Cache Observation

The image was built again without changing any files:

```bash
docker build -t cache-lab:v1 .
```

Docker was able to reuse previous build results, and cached steps appeared in the build output.

This demonstrated that Docker does not necessarily execute every Dockerfile instruction again when rebuilding an unchanged image.

---

## Cache Invalidation Experiment

The application source was then modified without changing `requirements.txt`.

For example:

```python
import requests

print("Docker Cache Lab - Application Updated")
print("Requests version:", requests.__version__)
```

The image was rebuilt:

```bash
docker build -t cache-lab:v1 .
```

Because the Dockerfile contained:

```dockerfile
COPY . .
```

the source change invalidated that `COPY` step.

Since dependency installation occurred after that step:

```dockerfile
RUN pip install --no-cache-dir -r requirements.txt
```

Docker could no longer safely reuse the same dependency-installation result.

The dependency installation therefore had to be evaluated again even though `requirements.txt` itself had not changed.

---

## Why the Original Dockerfile Was Inefficient

The original Dockerfile grouped files with very different rates of change into one operation.

```dockerfile
COPY . .
```

included both:

```text
requirements.txt
app.py
```

Application source code may change frequently while dependencies may remain unchanged for many builds.

By copying both together before dependency installation, a small application change can invalidate more build work than necessary.

---

## Experiment 2 — Optimized Dockerfile

The Dockerfile was reorganized so the dependency definition was copied first.

```dockerfile
FROM python:3.13-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

CMD ["python", "app.py"]
```

The new sequence became:

```text
Copy dependency definition
        ↓
Install dependencies
        ↓
Copy application source
```

The optimized image was built using:

```bash
docker build -t cache-lab:optimized .
```

It was then executed:

```bash
docker run --rm cache-lab:optimized
```

---

## Application Change After Optimization

Only `app.py` was modified again.

The optimized image was rebuilt:

```bash
docker build -t cache-lab:optimized .
```

This time Docker could reuse the earlier dependency-related steps because `requirements.txt` had not changed.

Conceptually:

```text
FROM python:3.13-slim
        ↓
CACHED

WORKDIR /app
        ↓
CACHED

COPY requirements.txt
        ↓
CACHED

RUN pip install
        ↓
CACHED

COPY app.py
        ↓
REBUILT
```

This demonstrated why stable dependencies should normally be separated from frequently changing source files in a Dockerfile.

---

## Experiment 3 — Dependency Change

The dependency file was then deliberately changed.

Example:

```text
requests==2.32.5
urllib3==2.5.0
```

The image was rebuilt:

```bash
docker build -t cache-lab:optimized .
```

Because `requirements.txt` changed, the dependency-related cache was correctly invalidated.

Docker therefore had to rerun:

```dockerfile
RUN pip install --no-cache-dir -r requirements.txt
```

This was expected behavior because the actual dependency definition had changed.

---

## Inspecting Image History

The image history was inspected using:

```bash
docker image history cache-lab:optimized
```

This command provided visibility into the instructions that contributed to the image and the approximate size associated with image layers.

The experiment reinforced that Docker images are assembled from multiple layers rather than being stored as one single monolithic filesystem.

---

## Key Findings

Docker build cache is most effective when the Dockerfile separates files according to how frequently they change.

A broad instruction such as:

```dockerfile
COPY . .
```

can invalidate more cache than necessary when it appears early in the Dockerfile.

A more efficient pattern is:

```dockerfile
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .
```

This allows dependency installation to remain cached while application source code changes.

The experiment also demonstrated that cache invalidation is not inherently bad. When the dependency file actually changes, Docker should rerun dependency installation.

The objective is not to avoid rebuilding entirely. The objective is to ensure that Docker only rebuilds the parts that actually need to change.

---

## Production Relevance

Docker build cache becomes increasingly important in CI/CD pipelines where images may be rebuilt repeatedly throughout the day.

Poor Dockerfile ordering can cause expensive operations such as dependency installation or compilation to run unnecessarily.

Better Dockerfile structure can reduce repeated work and improve build efficiency without changing application behavior.

The same principle applies across programming ecosystems.

For example:

```text
Python
requirements.txt before application source

Node.js
package.json / lock file before application source

Go
go.mod and go.sum before source code

Java
dependency metadata separated from frequently changing source
```

The implementation differs, but the caching principle remains the same.

---

## Commands Used

```bash
docker build -t cache-lab:v1 .

docker run --rm cache-lab:v1

docker build -t cache-lab:optimized .

docker run --rm cache-lab:optimized

docker image history cache-lab:optimized
```

---

## Engineering Takeaway

The most important lesson from this lab is:

> Dockerfile ordering is part of build performance engineering.

A working Dockerfile is not automatically an efficient Dockerfile.

Separating stable dependency definitions from frequently changing application source allows Docker to preserve useful cache layers and avoid unnecessary repeated work.