# Docker: Introduction and Images

## 1. What is Docker?
* **Definition:** Docker is an open-source platform designed to build, package, ship, and run applications in containers.
* **Container:** A container is a lightweight, standalone, executable software package that includes everything needed to run an application, including code, runtime, libraries, system tools, and configuration.
* **Goal:** It ensures that an app runs the same way everywhere, whether on a laptop, a server, or in the cloud.

### Why Do We Need Docker?
| Feature | Traditional Deployment | With Docker |
| :--- | :--- | :--- |
| **Dependencies** | Apps and dependencies are installed directly on the OS. | Each app runs in its own isolated container. |
| **Conflicts** | Different apps might require different library versions, causing conflicts. | Containers include all dependencies, creating a consistent environment everywhere. |
| **Deployment** | Deploying on a new machine often requires manual setup. | Containers can be easily started, stopped, copied, or removed. |

---

## 2. Key Concepts and Components
* **Image:** A read-only blueprint or template used to define a container. It contains the code, runtime, and system libraries.
* **Container:** A running instance of an image.
* **Dockerfile:** A text file containing instructions (commands) to build an image.
* **Docker Engine:** The runtime responsible for building and running containers.
* **Docker Hub:** A public repository for images, similar to GitHub for code.
* **Docker Compose:** A tool used to define and run multi-container applications.

---

## 3. Docker Architecture
Docker operates on a **Client-Server Model**.

1.  **Docker CLI (Client):** The command line interface (`docker build`, `docker pull`, `docker run`) that talks to the daemon.
2.  **Docker Daemon (Server):** A background service that does the heavy lifting, such as building, running, and managing containers.
3.  **Registry:** A storage location for images (e.g., Docker Hub).

**Flow:** The Client sends commands to the Daemon $\rightarrow$ The Daemon manages Images and Containers $\rightarrow$ It interacts with the Host Operating System.

---

## 4. Installation and Basic Commands

### Installation
* **Convenience Script:** `curl -fsSL https://get.docker.com -o get-docker.sh` followed by `sudo sh get-docker.sh`.
* **Linux (apt):** `sudo apt install docker.io -y`.
* **Verification:** Run `docker --version` or `docker run hello-world`.

### Common Commands
| Task | Command | Description |
| :--- | :--- | :--- |
| Check version | `docker --version` | Verify Docker installation. |
| Run container | `docker run hello-world` | Test the setup. |
| List active containers | `docker ps` | Show running containers. |
| List all containers | `docker ps -a` | Show all containers, including stopped ones. |
| List images | `docker images` | Show installed images. |
| Pull image | `docker pull <name>` | Download an image from Docker Hub. |
| Interactive mode | `docker run -it <image> bash` | Open a terminal inside the container. |
| Stop container | `docker stop <id>` | Gracefully stop a container. |
| Remove container | `docker rm <id>` | Delete a container. |
| Remove image | `docker rmi <id>` | Delete an image. |

---

## 5. Docker Images Deep Dive

### Layered File System (UnionFS)
Docker images are composed of multiple read-only layers stacked on top of one another using a union file system (e.g., OverlayFS).
* **Layers:** Each layer represents filesystem changes (add, modify, delete).
* **Creation:** Commands like `RUN`, `COPY`, or `ADD` in a Dockerfile create new layers.

**Visual Structure:**
1.  **Base Layer:** The OS (e.g., `FROM ubuntu:22.04`).
2.  **Read-Only Layers:** Added by instructions like `apt-get install` or `COPY`.
3.  **Writable Container Layer:** When a container starts, Docker adds a read-write layer on top. Any changes made while the container is running happen here.

### How UnionFS Works
* **Unified View:** Merges all layers into a single view.
* **Copy-on-Write:** If a file exists in multiple layers, the topmost copy is used. To modify a file, it is copied from the read-only layer to the writable layer first.

### Storage and Caching
* **Location:** On Linux, images and layers are typically stored in `/var/lib/docker/overlay2/`.
* **Inspect Layers:** Use `docker image inspect <name>` or `docker history <name>` to view layer details.
* **Caching:** Layers are identified by SHA256 hashes. If two images share the same base l