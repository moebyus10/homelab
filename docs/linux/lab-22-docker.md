# 🐳 Docker

## 🎯 Objective

The goal of this lab is to install Docker Engine on Ubuntu 26.04 and learn the fundamentals of container administration.

During this lab, the following concepts were covered:

- Installing Docker Engine
- Managing Docker services with systemd
- Configuring user access to Docker
- Managing Docker images and containers
- Running interactive containers
- Using Docker Compose
- Deploying a containerized web service
- Monitoring Docker resources

---

## 📦 Installing Docker Engine

Docker was installed using the official Docker repository.

Required packages were installed:

```bash
sudo apt install -y ca-certificates curl gnupg
```

The Docker GPG key was added:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

The official Docker repository was configured:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) \
$(. /etc/os-release && echo ${UBUNTU_CODENAME:-$VERSION_CODENAME}) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

The repository was updated:

```bash
sudo apt update
```

Docker Engine and required components were installed:

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## 🔎 Docker Installation Verification

Docker version was checked:

```bash
docker --version
```

**Result:**
```text
Docker version 29.7.1
```

The Docker service status was verified:

```bash
sudo systemctl status docker
```

**Result:**
```text
Active: active (running)
```

Docker is correctly installed and running.

---

## 👤 Configuring Docker User Access

The user was added to the Docker group:

```bash
sudo usermod -aG docker moebyus
```

After refreshing the session, Docker commands were available without sudo.

**Verification:**

```bash
docker ps
```

**Result:**
```text
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS   PORTS   NAMES
```

Docker permissions are correctly configured.

---

## 🧪 Testing Docker Installation

The official hello-world image was executed:

```bash
docker run hello-world
```

**Result:**
```text
Hello from Docker!

This message shows that your installation appears to be working correctly.
```

Docker successfully:
- Connected to the Docker daemon
- Downloaded an image from Docker Hub
- Created a container
- Executed the container

---

## 📦 Managing Containers

All containers were listed:

```bash
docker ps -a
```

**Example result:**
```text
CONTAINER ID   IMAGE         COMMAND    STATUS
9565cf600bd9   hello-world   "/hello"   Exited (0)
```

The container was removed:

```bash
docker rm $(docker ps -aq)
```

The container list was verified:

```bash
docker ps -a
```

No containers remained.

---

## 🐧 Running an Ubuntu Container

An interactive Ubuntu container was created:

```bash
docker run -it --name ubuntu-test ubuntu bash
```

The container operating system was verified:

```bash
cat /etc/os-release
```

**Result:**
```text
PRETTY_NAME="Ubuntu 26.04 LTS"
VERSION="26.04 LTS (Resolute Raccoon)"
```

The container hostname was checked:

```bash
hostname
```

**Result:**
```text
c04fd9cb4100
```

The container was exited with:

```bash
exit
```

---

## 🔄 Container Lifecycle Management

The container state was checked:

```bash
docker ps -a
```

The container was started:

```bash
docker start ubuntu-test
```

Running containers were displayed:

```bash
docker ps
```

The container was stopped:

```bash
docker stop ubuntu-test
```

The final state was verified:

```bash
docker ps -a
```

The container lifecycle was successfully tested.

---

## 🖼️ Docker Image Management

Available images were listed:

```bash
docker images
```

The Ubuntu container was removed:

```bash
docker rm ubuntu-test
```

The Ubuntu image was deleted:

```bash
docker rmi ubuntu
```

**Verification:**

```bash
docker images
```

**Remaining images:**
```text
hello-world:latest
```

Image management was successfully tested.

---

## 🧩 Docker Compose

Docker Compose version was checked:

```bash
docker compose version
```

**Result:**
```text
Docker Compose version v5.3.1
```

A working directory was created:

```bash
mkdir ~/docker-lab
cd ~/docker-lab
```

A Compose configuration (`docker-compose.yml`) was created:

```yaml
services:
  web:
    image: nginx:latest
    container_name: nginx-lab
    ports:
      - "8080:80"
    restart: unless-stopped
```

The service was deployed:

```bash
docker compose up -d
```

---

## 🌐 Deploying Nginx Container

The running service was checked:

```bash
docker compose ps
```

The container was available through:
`http://localhost:8080`

The web server response was tested:

```bash
curl http://localhost:8080
```

Nginx successfully returned the default web page.

---

## 📋 Container Logs

Nginx logs were reviewed:

```bash
docker compose logs
```

**Logs confirmed:**
```text
Configuration complete; ready for start up
start worker processes
GET / HTTP/1.1 200
```

The web service was running correctly.

---

## 🧹 Stopping Docker Compose Service

The Compose deployment was stopped:

```bash
docker compose down
```

**Result:**
```text
Container nginx-lab Removed
Network docker-lab_default Removed
```

Running containers were verified:

```bash
docker ps
```

No containers were active.

---

## 📊 Docker System Information

Docker configuration was checked:

```bash
docker info
```

**Environment:**
```text
Docker Engine: 29.7.1
Docker Compose: v5.3.1
Operating System: Ubuntu 26.04 LTS
Kernel: Linux 7.0.0-28-generic
Architecture: x86_64
Storage Driver: overlayfs
Cgroup Version: 2
```

Docker disk usage was checked:

```bash
docker system df
```

**Result:**
```text
Images        2        240.6MB
Containers    0        0B
Volumes       0        0B
