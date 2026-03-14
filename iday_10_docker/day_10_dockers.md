[](./docker_process.png)
====================
## 🐳 Docker Hub vs Docker Registry

Both are used to **store and distribute Docker container images**, but they are slightly different in concept.

| Feature    | **Docker Hub**                                       | **Docker Registry**                                  |
| ---------- | ---------------------------------------------------- | ---------------------------------------------------- |
| Definition | A **public cloud service** for storing Docker images | A **system/service** that stores Docker images       |
| Type       | Hosted registry provided by Docker                   | Generic registry (can be private or public)          |
| Provider   | Managed by Docker Inc.                               | Can be hosted by anyone                              |
| Access     | Public images + private repositories                 | Usually private for organizations                    |
| Setup      | No setup required                                    | Must be installed/configured                         |
| Example    | hub.docker.com                                       | Private registry, AWS ECR, GitHub Container Registry |

---

## 🐳 Docker Hub

**Docker Hub** is the **default public registry** for Docker images.

### Key Points

* Stores **public and private Docker images**
* Used by default when running `docker pull`
* Contains official images like:

  * Ubuntu
  * Nginx
  * Node.js
* No infrastructure needed

Example:

```bash
docker pull nginx
```

This pulls the image from **Docker Hub automatically**.

---

## 📦 Docker Registry

A **Docker Registry** is the **general service that stores container images**.

Example registries:

* Amazon Elastic Container Registry
* Google Container Registry
* GitHub Container Registry

Organizations often run **private registries** to store internal images.

Example private registry usage:

```bash
docker push mycompany-registry.com/app:v1
```

---

## 🔑 Simple Way to Remember

**Docker Hub**

* Public website for Docker images
* Like **GitHub for container images**

**Docker Registry**

* The **storage system for container images**
* Can be **public or private**

---

✅ **One-line difference**

**Docker Hub = a public Docker registry hosted by Docker.**
**Docker Registry = any server/service that stores Docker images.**

---

=====================================
## docker file ==> docker image ==> conatiner bnta hia 

==================================
The highlighted line says:

> **“Docker Engine is the core component that runs containers. It is a client-server application with three major components.”**

Let’s break this down in simple terms.

---

## 1️⃣ What is Docker Engine?

**Docker Engine** is the main software that **creates, runs, and manages containers**.

Think of it as the **runtime system for containers**.
Without Docker Engine, you cannot run Docker containers.

📦 Example idea:

* You build a container image for a web app.
* **Docker Engine runs that image as a container.**

So in short:

**Docker Engine = Software that runs and manages containers.**

---

## 2️⃣ Why is it called a *Client–Server Application*?

Docker works using a **client-server architecture**.

This means there are two sides:

### Client

The **Docker CLI (`docker`)** where users type commands.

Example commands:

```bash
docker run nginx
docker ps
docker build .
```

### Server

The **Docker daemon** that actually performs the work.

It:

* creates containers
* runs containers
* manages images
* handles networking and storage

The client sends requests to the server.

---

## 3️⃣ The Three Major Components

### 1️⃣ Docker Daemon (`dockerd`)

Background service that manages:

* containers
* images
* networks
* volumes

It listens for Docker API requests.

⚠️ Note:
Modern Docker uses **`containerd` internally** to run containers, but **dockerd is still part of Docker Engine**.

---

### 2️⃣ Docker CLI (`docker`)

The **command-line tool** used by users.

Example:

```bash
docker run ubuntu
```

The CLI sends the request to the Docker daemon.

---

### 3️⃣ REST API

Allows **programs or tools** to communicate with Docker.

Example:

* Kubernetes
* Docker Desktop
* CI/CD tools

They use HTTP APIs to interact with the Docker daemon.

---

✅ **Simple Architecture**

```
User
  │
  ▼
Docker CLI (client)
  │
  ▼
Docker REST API
  │
  ▼
Docker Daemon (dockerd)
  │
  ▼
containerd → runs containers
```

---

✅ **One-line exam definition:**

**Docker Engine is the core runtime that runs and manages containers using a client-server architecture consisting of Docker CLI, Docker daemon, and REST API.**

---

====================================
This slide explains **Docker Networking** and the **default network types**. Let’s simplify it so it’s easy to understand for exams and interviews.

---

# 7. Docker Networking

## 1️⃣ Network Isolation

By default, **Docker containers are isolated from each other**.

That means:

* Containers cannot automatically communicate with each other.
* Docker provides **networks** to control communication.

So you decide:

* Which containers can talk to each other
* Which containers can access the outside world 🌐

📌 Example
A system with:

* Web container
* Database container

Docker networking lets you **connect the web container to the database container securely**.

---

# 2️⃣ Default Docker Network Types

## 🔹 Bridge Network (Default)

The **bridge network** is the default network used for standalone containers.

How it works:

* Docker creates a **virtual network inside the host**
* Containers connected to it can communicate using **IP or container names**

Example:

```bash
docker run -d --name web nginx
docker run -d --name db mysql
```

Both containers run on the **bridge network**.

📌 Key points:

* Default network
* Used for **single-host containers**
* Containers communicate internally

---

## 🔹 Host Network

In **host mode**, the container shares the **host machine's network stack**.

This means:

* No separate container IP
* Container uses the **host's IP and ports**

Example:

```bash
docker run --network host nginx
```

📌 Key points:

* No network isolation
* Faster networking
* Uses host ports directly

Example:
If nginx runs on port **80**, it runs directly on the host.

---

## 🔹 Overlay Network

Used for **multi-host container communication**.

Mainly used in:

* **Docker Swarm**
* Distributed container systems

It allows containers on **different machines** to communicate as if they are on the same network.

Example:

```
Server 1 → Container A
Server 2 → Container B
```

Overlay network connects them.

📌 Key points:

* Multi-host networking
* Used in **Docker Swarm clusters**

---

## 🔹 None Network

The container has **no network interface**.

Example:

```bash
docker run --network none nginx
```

📌 Result:

* No internet
* No communication
* Fully isolated container

Used for:

* Security
* Testing
* Special workloads

---

# 🧠 Quick Exam Summary

| Network Type | Description                               |
| ------------ | ----------------------------------------- |
| Bridge       | Default network for standalone containers |
| Host         | Container shares host network             |
| Overlay      | Multi-host networking (Docker Swarm)      |
| None         | No networking for the container           |

---

✅ **One-line interview answer:**

**Docker networking allows containers to communicate with each other and external systems using networks such as Bridge, Host, Overlay, and None.**

---

===================================
This slide lists **Common Docker Commands**. Here is a **simple explanation with examples** so it’s easy to remember.

---

# 11. Common Docker Commands

## 1️⃣ `docker build`

**Builds a Docker image from a Dockerfile.**

A **Dockerfile** contains instructions to create an image.

Example:

```bash
docker build -t myapp .
```

Explanation:

* `-t myapp` → name of the image
* `.` → Dockerfile location (current directory)

📌 Result: Creates a **Docker image**.

---

## 2️⃣ `docker run`

**Runs a container from a Docker image.**

Example:

```bash
docker run nginx
```

Docker will:

1. Pull the image (if not present)
2. Create a container
3. Start the container

Example with port mapping:

```bash
docker run -p 8080:80 nginx
```

---

## 3️⃣ `docker ps`

**Lists running containers.**

Example:

```bash
docker ps
```

Output shows:

* Container ID
* Image
* Status
* Ports
* Names

To see **all containers (including stopped ones):**

```bash
docker ps -a
```

---

## 4️⃣ `docker stop`

**Stops a running container.**

Example:

```bash
docker stop container_id
```

Example:

```bash
docker stop my_container
```

---

## 5️⃣ `docker rm`

**Removes a stopped container.**

Example:

```bash
docker rm container_id
```

Example:

```bash
docker rm my_container
```

⚠️ Container must be **stopped before removing**.

---

## 6️⃣ `docker pull`

**Downloads an image from a registry (usually Docker Hub).**

Example:

```bash
docker pull ubuntu
```

This downloads the **Ubuntu image** from Docker Hub.

---

## 7️⃣ `docker push`

**Uploads an image to a registry (like Docker Hub).**

Example:

```bash
docker push username/myapp
```

Used when sharing images or deploying applications.

---

# 🧠 Quick Memory Trick

| Command      | Purpose          |
| ------------ | ---------------- |
| docker build | Create image     |
| docker run   | Run container    |
| docker ps    | List containers  |
| docker stop  | Stop container   |
| docker rm    | Remove container |
| docker pull  | Download image   |
| docker push  | Upload image     |

---

✅ **Short interview answer:**

Common Docker commands include **docker build, docker run, docker ps, docker stop, docker rm, docker pull, and docker push**, which are used to build images, run containers, manage containers, and interact with Docker registries.

---

========================================
## commands
These slides show the **sequence of commonly used Docker commands** — basically the **step-by-step workflow of using Docker**. I’ll simplify it into an easy flow so you understand the **real order used in practice**.

---

# Sequence of Commonly Used Docker Commands

## 1️⃣ Install Docker (Ubuntu)

First install Docker:

```bash
sudo apt update
sudo apt install docker.io
sudo systemctl enable --now docker
```

Explanation:

* `apt update` → updates package list
* `apt install docker.io` → installs Docker
* `systemctl enable --now docker` → starts Docker service and enables it at boot

---

# 2️⃣ Verify Docker Installation

Check if Docker installed correctly:

```bash
docker --version
```

Example output:

```
Docker version 24.x.x
```

---

# 3️⃣ Check Docker Service Status

```bash
sudo systemctl status docker
```

Shows whether the **Docker service is running**.

---

# 4️⃣ Pull an Image from Docker Hub

Example: Pull **Nginx image**

```bash
docker pull nginx
```

What happens:

* Docker downloads the **nginx image** from **Docker Hub registry**.

---

# 5️⃣ List Downloaded Images

```bash
docker images
```

Example output:

```
REPOSITORY   TAG     IMAGE ID
nginx        latest  605c77e6
```

---

# 6️⃣ Run Container Interactively

```bash
docker run -it nginx /bin/bash
```

Explanation:

* `-i` → interactive mode
* `-t` → terminal
* `/bin/bash` → open bash shell inside container

Used mainly for **testing or debugging**.

---

# 7️⃣ Run Container in Background (Detached Mode)

```bash
docker run -d --name my-nginx-container nginx
```

Explanation:

* `-d` → detached mode (background)
* `--name` → container name
* `nginx` → image name

Now nginx container runs **in the background**.

---

# 8️⃣ List Running Containers

```bash
docker ps
```

Shows:

* container ID
* image
* status
* ports
* names

---

# 9️⃣ List All Containers

Including stopped ones:

```bash
docker ps -a
```

---

# 🔟 Stop a Container

```bash
docker stop my-nginx-container
```

Stops the running container.

---

# 1️⃣1️⃣ Start a Stopped Container

```bash
docker start my-nginx-container
```

Restarts the stopped container.

---

# 1️⃣2️⃣ Remove a Container

Container must be **stopped first**.

```bash
docker rm my-nginx-container
```

Deletes the container.

---

# 1️⃣3️⃣ Remove an Image

```bash
docker rmi nginx
```

Deletes the nginx image.

---

# 1️⃣4️⃣ Build Custom Image

If you have a **Dockerfile**:

```bash
docker build -t my-nginx-image .
```

Explanation:

* `-t` → tag (image name)
* `.` → Dockerfile location

---

# 1️⃣5️⃣ Tag an Image

Used before pushing to a repository.

```bash
docker tag my-nginx-image my-repo/my-nginx-image:v1
```

Example:

* `my-repo` → Docker Hub username
* `v1` → version tag

---

# 🧠 Full Docker Workflow (Real DevOps Flow)

```
Install Docker
      ↓
Verify Installation
      ↓
Pull Image
      ↓
Run Container
      ↓
Check Running Containers
      ↓
Stop / Start Container
      ↓
Remove Container
      ↓
Build Custom Image
      ↓
Tag Image
      ↓
Push to Docker Hub
```

---

✅ **Short Interview Answer**

The typical Docker workflow includes installing Docker, pulling images, running containers, listing containers, stopping/starting containers, removing containers, building custom images, tagging images, and pushing them to a registry.

---

This slide shows the **next Docker commands in the workflow**: pushing images to Docker Hub, inspecting containers, and viewing logs. Here’s a clear explanation.

---

# Push Image to Docker Hub

Before pushing an image, you must **log in to Docker Hub**.

### 1️⃣ Login to Docker Hub

```bash
docker login
```

It will ask for:

* Docker Hub **username**
* **password**

After successful login, you can push images.

---

### 2️⃣ Push the Image

```bash
docker push my-repo/my-nginx-image:v1
```

Explanation:

* `my-repo` → Docker Hub username or repository
* `my-nginx-image` → image name
* `v1` → version tag

📦 Result:
Your custom image is uploaded to **Docker Hub registry** so others (or your servers) can download it.

Example workflow:

```
Build Image
      ↓
Tag Image
      ↓
Login Docker Hub
      ↓
Push Image
```

---

# Inspect a Container

Command:

```bash
docker inspect my-nginx-container
```

This command shows **detailed information about a container** in JSON format.

It includes:

* container ID
* network settings
* IP address
* mounted volumes
* configuration
* status

Example use:
If you want to find the **container IP address**.

---

# View Container Logs

To see logs generated by a container:

```bash
docker logs my-nginx-container
```

Example output for nginx:

```
192.168.1.5 - - [request log]
```

Useful for:

* debugging applications
* checking server activity
* troubleshooting errors

You can also watch logs live:

```bash
docker logs -f my-nginx-container
```

`-f` = follow logs in real time.

---

# Quick Summary Table

| Command          | Purpose                  |
| ---------------- | ------------------------ |
| `docker login`   | Login to Docker Hub      |
| `docker push`    | Upload image to registry |
| `docker inspect` | Detailed container info  |
| `docker logs`    | View container logs      |

---

✅ **Simple interview explanation:**

`docker push` uploads an image to Docker Hub, `docker inspect` shows detailed container information, and `docker logs` displays logs generated by a container for debugging.

---

This slide explains **three important Docker concepts** used when running containers:

1️⃣ **Port Mapping**
2️⃣ **Volume Mapping**
3️⃣ **Executing commands inside a container**

Let’s break them down.

---

# 1️⃣ Port Mapping in Docker

Command shown:

```bash
docker run -d -p 8080:80 nginx
```

Explanation:

* `-d` → run container in **detached (background) mode**
* `-p 8080:80` → **port mapping**
* `nginx` → image name

📌 Meaning of `8080:80`

```
Host Port : Container Port
```

```
Your Computer (Host)
        │
localhost:8080
        │
        ▼
Docker Container
        │
       Port 80 (Nginx)
```

So when you open:

```
http://localhost:8080
```

You will see the **Nginx web server running inside the container**.

---

# 2️⃣ Volume Mapping (Mounting Local Directory)

Command:

```bash
docker run -d -v /path/to/host/directory:/usr/share/nginx/html -p 8080:80 nginx
```

Explanation:

* `-v` → volume mapping
* `/path/to/host/directory` → folder on your system
* `/usr/share/nginx/html` → nginx web directory inside container

📌 What happens:

```
Host Folder
/path/to/host/directory
        │
        ▼
Container Folder
/usr/share/nginx/html
```

If you add an HTML file in your **host folder**, nginx inside the container will serve it.

Example:

```
host folder:
index.html
```

Open:

```
http://localhost:8080
```

Your HTML page will appear.

💡 This is how **developers serve websites using Docker**.

---

# 3️⃣ Execute Command Inside Running Container

Command:

```bash
docker exec -it my-nginx-container /bin/bash
```

Explanation:

* `docker exec` → run command inside container
* `-i` → interactive
* `-t` → terminal
* `my-nginx-container` → container name
* `/bin/bash` → open bash shell

📌 Result:

You enter the container terminal:

```
root@container:/#
```

Now you can run Linux commands **inside the container**.

Example:

```
ls
cd /usr/share/nginx/html
```

---

# 4️⃣ View Container Logs

Command:

```bash
docker logs my-nginx-container
```

Shows logs generated by the container.

Useful for:

* debugging
* monitoring
* checking server activity

---

# 🧠 Quick Summary

| Command                               | Purpose                         |
| ------------------------------------- | ------------------------------- |
| `docker run -p 8080:80 nginx`         | Map host port to container      |
| `docker run -v host:container`        | Mount local folder to container |
| `docker exec -it container /bin/bash` | Enter container terminal        |
| `docker logs container`               | View container logs             |

---

✅ **Simple interview explanation:**

Docker allows **port mapping to expose container services, volume mapping to share data between host and container, and docker exec to interact with running containers.**

---

=========================================
This slide shows **Docker cleanup commands** used to remove **unused containers, images, volumes, networks, and build cache**. These commands help **free disk space and keep Docker clean**.

---

# Docker Cleanup Commands (Prune Commands)

In Docker, **`prune` means removing unused resources**.

---

# 1️⃣ `docker system prune`

Command:

```bash
docker system prune
```

Removes:

* stopped containers
* unused networks
* dangling images
* build cache

📌 **Dangling images** = images without a tag or not used by containers.

Example output:

```
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - dangling images
```

Docker will ask:

```
Are you sure you want to continue? [y/N]
```

---

# 2️⃣ `docker system prune -a`

Command:

```bash
docker system prune -a
```

This is **more aggressive cleanup**.

Removes:

* stopped containers
* unused networks
* **ALL unused images** (not just dangling)

⚠️ Use carefully because it may remove images you still need.

---

# 3️⃣ `docker container prune`

Command:

```bash
docker container prune
```

Removes:

* **all stopped containers**

Example:

```
Deleted Containers:
abc123
def456
```

---

# 4️⃣ `docker image prune`

Command:

```bash
docker image prune
```

Removes:

* **dangling images only**

Meaning:

* images without tags
* unused intermediate images

---

# 5️⃣ `docker image prune -a`

Command:

```bash
docker image prune -a
```

Removes:

* **all unused images**
* images not used by any container

---

# 6️⃣ `docker volume prune`

Command:

```bash
docker volume prune
```

Removes:

* **unused volumes**

Volumes store **persistent container data**.

Unused volumes can consume **large disk space**.

---

# 7️⃣ `docker network prune`

Command:

```bash
docker network prune
```

Removes:

* **unused Docker networks**

Networks not connected to containers will be deleted.

---

# 8️⃣ `docker builder prune`

Command:

```bash
docker builder prune
```

Removes:

* **build cache** created during image builds.

When building images repeatedly, cache grows large.

This command cleans it.

---

# 🧠 Quick Summary Table

| Command                  | What It Removes                       |
| ------------------------ | ------------------------------------- |
| `docker system prune`    | containers, networks, dangling images |
| `docker system prune -a` | all unused images + containers        |
| `docker container prune` | stopped containers                    |
| `docker image prune`     | dangling images                       |
| `docker image prune -a`  | all unused images                     |
| `docker volume prune`    | unused volumes                        |
| `docker network prune`   | unused networks                       |
| `docker builder prune`   | build cache                           |

---

✅ **Simple Interview Explanation**

Docker prune commands are used to clean unused Docker resources such as containers, images, volumes, networks, and build cache to free disk space.

---

💡 **Most commonly used in real projects:**

```bash
docker system prune -a
```

This quickly **cleans the entire Docker environment**.

---

