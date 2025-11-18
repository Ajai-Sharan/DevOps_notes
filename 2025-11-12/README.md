# Creating Docker Image — Notes
## What Is a Docker Image?

A Docker image is a read-only, layered filesystem.

Acts as a blueprint for containers.

Running an image = creating a container with a writable layer on top.

## Dockerfile Fundamentals
### 1. FROM — Base Image

Specifies the starting image.

Always the first instruction (except ARG).

Best practices:

Use slim/minimal images (-alpine, -slim)

Pin versions (python:3.12-slim)

### 2. RUN — Execute Commands During Build

Used to install software / configure environment.

Creates a new image layer.

Best Practices:

Combine commands to reduce layers.

Clean caches.

Use multi-stage builds.

### 3. CMD — Default Command

Command executed when container starts.

Only one CMD allowed.

Use JSON form.

### 4. ENTRYPOINT — Fixed Main Command

Defines the main executable of the container.

Use CMD to provide default args.

### 5. COPY vs ADD
|COPY |	ADD |
|Standard file copy | Supports remote URLs & auto-extracts tars|
|Preferred | Use only when needed |
### 6. ENV — Environment Variables

Example:

ENV PORT=8080

## Real-World Example – Flask App Dockerfile
```
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
ENV PORT=8080
EXPOSE 8080
ENTRYPOINT ["python"]
CMD ["app.py"] 
```

## Docker Image Best Practices

Use minimal base images.

Combine layers.

Don’t hardcode secrets.

Use non-root users.

Add HEALTHCHECK.

Use multi-stage builds.

## Inspecting Images
```
docker history 
docker inspect 
docker scan
```