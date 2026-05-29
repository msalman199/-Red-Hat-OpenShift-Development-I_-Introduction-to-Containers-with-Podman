# 🚀 Running Containers with Podman

<p align="center">
  <img src="https://img.shields.io/badge/Podman-Container_Engine-purple?style=for-the-badge&logo=podman">
  <img src="https://img.shields.io/badge/Containers-Management-blue?style=for-the-badge&logo=docker">
  <img src="https://img.shields.io/badge/Linux-Administration-black?style=for-the-badge&logo=linux">
  <img src="https://img.shields.io/badge/DevOps-Containerization-orange?style=for-the-badge&logo=kubernetes">
</p>

---

# 📘 Overview

## 🎯 Objectives

By the end of this lab, you will be able to:

✅ Run containers in detached mode  
✅ Map container ports to host ports  
✅ Mount host directories into containers  
✅ Assign custom names to containers  

---

# 🛠️ Prerequisites

Before starting this lab, ensure you have:

🔹 Podman Installed (version 3.0+)  
🔹 Basic Command Line Knowledge  
🔹 Linux-based System (Recommended)  

---

# ⚙️ Lab Setup

# 🔹 Verify Podman Installation

```bash
podman --version
```

---

# ✅ Expected Output

```bash
podman version 3.4.4
```

---

# 🔹 Pull Nginx Container Image

```bash
podman pull docker.io/library/nginx:alpine
```

---

# 🚀 Task 1 — Running a Container in Detached Mode

## 🎯 Objective

Learn how to run containers in the background.

---

# 🔹 Step 1 — Run Nginx Container

```bash
podman run -d docker.io/library/nginx:alpine
```

---

# 📖 Key Concept

| Option | Description |
|---|---|
| `-d` | Run container in detached mode |

---

# 🔹 Step 2 — Verify Running Container

```bash
podman ps
```

---

# ✅ Expected Output

Displays running container with auto-generated name.

---

# 🛠️ Troubleshooting

If container fails to start:

```bash
podman logs <container_id>
```

---

# 🌐 Task 2 — Port Mapping

## 🎯 Objective

Learn to map container ports to host ports.

---

# 🔹 Step 1 — Stop Running Containers

```bash
podman stop $(podman ps -q)
```

---

# 🔹 Step 2 — Run Container with Port Mapping

```bash
podman run -d -p 8080:80 docker.io/library/nginx:alpine
```

---

# 📖 Key Concept

| Option | Description |
|---|---|
| `-p` | Port Mapping |
| `8080` | Host Port |
| `80` | Container Port |

---

# 🔹 Step 3 — Verify Port Mapping

```bash
podman port <container_id>
```

---

# ✅ Expected Output

```bash
80/tcp -> 0.0.0.0:8080
```

---

# 🔹 Step 4 — Test Container Access

```bash
curl http://localhost:8080
```

---

# ✅ Expected Result

Returns Nginx welcome page HTML.

---

# 🛠️ Troubleshooting

If port conflict occurs:

```bash
ss -tulnp
```

Choose another host port.

---

# 📂 Task 3 — Volume Mounts

## 🎯 Objective

Learn to mount host directories into containers.

---

# 🔹 Step 1 — Create Host Directory

```bash
mkdir ~/nginx-content

echo "Hello from host!" > ~/nginx-content/index.html
```

---

# 🔹 Step 2 — Run Container with Volume Mount

```bash
podman run -d -p 8081:80 -v ~/nginx-content:/usr/share/nginx/html:Z docker.io/library/nginx:alpine
```

---

# 📖 Key Concept

| Option | Description |
|---|---|
| `-v` | Volume Mount |
| `host_path` | Host Directory |
| `container_path` | Container Directory |
| `:Z` | SELinux Context |

---

# 🔹 Step 3 — Verify Mounted Content

```bash
curl http://localhost:8081
```

---

# ✅ Expected Output

```bash
Hello from host!
```

---

# 🛠️ Troubleshooting

If permission issues occur:

```bash
chcon -Rt svirt_sandbox_file_t ~/nginx-content
```

---

# 🏷️ Task 4 — Assigning Custom Names

## 🎯 Objective

Learn to assign custom names to containers.

---

# 🔹 Step 1 — Run Named Container

```bash
podman run -d --name my-nginx -p 8082:80 docker.io/library/nginx:alpine
```

---

# 📖 Key Concept

| Option | Description |
|---|---|
| `--name` | Assign Custom Name |

---

# 🔹 Step 2 — Verify Container Status

```bash
podman inspect my-nginx | grep -i status
```

---

# ✅ Expected Output

Shows container status as:

```bash
running
```

---

# 🔹 Step 3 — Stop Container Using Name

```bash
podman stop my-nginx
```

---

# 🛠️ Troubleshooting

If container name already exists:

✅ Remove old container  
✅ Or choose another name  

---

# 📊 Common Podman Commands Cheat Sheet

| Command | Description |
|---|---|
| `podman ps` | List running containers |
| `podman ps -a` | List all containers |
| `podman run` | Run container |
| `podman stop` | Stop container |
| `podman rm` | Remove container |
| `podman logs` | View logs |
| `podman inspect` | Inspect container |
| `podman port` | View port mapping |

---

# ⚡ Benefits of Podman

✅ Daemonless Architecture  
✅ Rootless Containers  
✅ Improved Security  
✅ Lightweight  
✅ OCI Compatible  
✅ Docker-Compatible Commands  

---

# 🧹 Cleanup Containers

## 🔹 Remove All Containers

```bash
podman rm -f $(podman ps -aq)
```

---

# 📚 Explore Podman Help

```bash
podman --help
```

---

# 🧰 Technologies Used

<p align="center">

<img src="https://img.shields.io/badge/Podman-Container_Engine-purple?style=for-the-badge&logo=podman">

<img src="https://img.shields.io/badge/Docker-Containers-blue?style=for-the-badge&logo=docker">

<img src="https://img.shields.io/badge/Linux-Administration-black?style=for-the-badge&logo=linux">

<img src="https://img.shields.io/badge/Kubernetes-Orchestration-blue?style=for-the-badge&logo=kubernetes">

<img src="https://img.shields.io/badge/OpenShift-Container_Platform-red?style=for-the-badge&logo=redhatopenshift">

<img src="https://img.shields.io/badge/DevOps-Automation-orange?style=for-the-badge&logo=azuredevops">

</p>

---

# 🎓 Conclusion

In this lab, you learned:

✅ Running containers in detached mode  
✅ Mapping ports between host and container  
✅ Mounting host directories  
✅ Assigning custom container names  
✅ Inspecting and managing containers  

These are essential skills for working with containers in real-world production environments.

---

# 🚀 Next Steps

🔹 Explore Podman Networking  
🔹 Learn Container Volumes  
🔹 Study Kubernetes Basics  
🔹 Deploy Multi-Container Applications  

---

# 📖 Additional Resources

## 🌐 Podman Official Documentation

https://podman.io/docs

---

## 🌐 Red Hat Container Training

https://www.redhat.com/en/services/training

---

# 👨‍💻 Author

## Hafiz Muhammad Salman

📧 hafizmuhammadsalman13@gmail.com  
🌐 GitHub: https://github.com/msalman199  
📱 +92 314 3563640  

---

# ⭐ Happy Learning Containers & DevOps ⭐
