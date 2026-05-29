# 🚀 Creating and Managing Pods with Podman

<p align="center">
  <img src="https://img.shields.io/badge/Podman-Pods-purple?style=for-the-badge&logo=podman">
  <img src="https://img.shields.io/badge/Containers-Orchestration-blue?style=for-the-badge&logo=docker">
  <img src="https://img.shields.io/badge/Linux-System_Administration-black?style=for-the-badge&logo=linux">
  <img src="https://img.shields.io/badge/DevOps-Containerization-orange?style=for-the-badge&logo=kubernetes">
</p>

---

# 📘 Overview

## 🎯 Objectives

By the end of this lab, you will be able to:

✅ Understand the concept of Pods  
✅ Create and manage Pods using Podman  
✅ Run multiple containers inside a Pod  
✅ Inspect Pod networking and shared volumes  

---

# 🛠️ Prerequisites

Before starting this lab, ensure you have:

🔹 Linux-based system (Ubuntu 20.04+ or CentOS 8+)  
🔹 Podman installed  
🔹 Basic container knowledge  
🔹 Rootless Podman configured  

---

# ⚙️ Verify Podman Installation

```bash
podman --version
```

---

# 🔍 Verify Rootless Podman

```bash
podman info
```

---

# 🏗️ Task 1 — Creating a Pod

---

# 🔹 Subtask 1.1 — Create Basic Pod

## 🚀 Create Pod

```bash
podman pod create --name demo-pod -p 8080:80
```

---

# 📖 Command Explanation

| Option | Description |
|---|---|
| `podman pod create` | Create Pod |
| `--name demo-pod` | Assign Pod Name |
| `-p 8080:80` | Map Host Port to Pod Port |

---

# 🔹 Verify Pod Creation

```bash
podman pod list
```

---

# ✅ Expected Output

```bash
POD ID        NAME       STATUS    CREATED         INFRA ID      # OF CONTAINERS
abc123        demo-pod   Created   2 minutes ago   def456        1
```

---

# 🔹 Subtask 1.2 — Add Container to Pod

## 🌐 Run Nginx Container Inside Pod

```bash
podman run -d --pod demo-pod --name nginx-container docker.io/library/nginx:alpine
```

---

# 📖 Command Explanation

| Option | Description |
|---|---|
| `-d` | Detached Mode |
| `--pod demo-pod` | Add Container to Pod |
| `--name` | Container Name |

---

# 🔹 Verify Running Containers

```bash
podman ps --pod
```

---

# ✅ Expected Outcome

Displays:

✅ Infra Container  
✅ nginx-container  

---

# 🚀 Task 2 — Running Multiple Containers in a Pod

---

# 🔹 Subtask 2.1 — Add Sidecar Container

## 🗄️ Run Redis Container

```bash
podman run -d --pod demo-pod --name redis-container docker.io/library/redis:alpine
```

---

# 🔹 Verify Both Containers

```bash
podman pod inspect demo-pod | jq '.Containers[].Names'
```

---

# ✅ Expected Output

```bash
[
  "nginx-container"
]

[
  "redis-container"
]
```

---

# 🔹 Subtask 2.2 — Verify Shared Networking

## 🖥️ Access Nginx Container

```bash
podman exec -it nginx-container sh
```

---

# 🔹 Ping Redis Container

```bash
ping redis-container
```

---

# 🔑 Key Concept

Containers inside the same Pod:

✅ Share Network Namespace  
✅ Can Communicate Using Hostnames  
✅ Can Access Each Other via localhost  

---

# 📡 Task 3 — Inspecting Pod Networking and Volumes

---

# 🔹 Subtask 3.1 — Network Inspection

## 🌐 Check Pod Network Information

```bash
podman pod inspect demo-pod | jq '.InfraConfig.NetworkOptions'
```

---

# 🔹 View Port Mappings

```bash
podman port demo-pod
```

---

# ✅ Expected Output

```bash
80/tcp -> 0.0.0.0:8080
```

---

# 🔹 Subtask 3.2 — Create Shared Volume

## 📂 Create Shared Volume

```bash
podman volume create shared-vol
```

---

# 🔹 Run Nginx Container with Shared Volume

```bash
podman run -d --pod demo-pod --name nginx2 \
-v shared-vol:/data docker.io/library/nginx:alpine
```

---

# 🔹 Run Redis Container with Shared Volume

```bash
podman run -d --pod demo-pod --name redis2 \
-v shared-vol:/data docker.io/library/redis:alpine
```

---

# 🔹 Verify Shared Volume

## ✍️ Create File Inside nginx2

```bash
podman exec -it nginx2 touch /data/testfile
```

---

# 🔹 Verify File Inside redis2

```bash
podman exec -it redis2 ls /data
```

---

# ✅ Expected Output

```bash
testfile
```

---

# 🔑 Key Concepts

| Feature | Description |
|---|---|
| Shared Network | Containers communicate internally |
| Shared Volume | Multiple containers access same data |
| Infra Container | Maintains Pod namespaces |

---

# 🛠️ Troubleshooting Tips

---

# ❌ Pod Fails to Start

Check logs:

```bash
podman logs <container_name>
```

---

# 🌐 Network Issues

Verify firewall rules:

```bash
sudo firewall-cmd --list-ports
```

---

# 🧹 Remove Failed Pod

```bash
podman pod rm -f demo-pod
```

---

# 📊 Common Podman Pod Commands

| Command | Description |
|---|---|
| `podman pod create` | Create Pod |
| `podman pod list` | List Pods |
| `podman pod inspect` | Inspect Pod |
| `podman pod rm` | Remove Pod |
| `podman ps --pod` | Show Pod Containers |
| `podman exec` | Execute Command in Container |
| `podman volume create` | Create Volume |

---

# ⚡ Benefits of Pods

✅ Shared Networking  
✅ Shared Storage  
✅ Easier Container Management  
✅ Multi-Container Applications  
✅ Kubernetes-Compatible Architecture  

---

# 🧹 Cleanup Environment

## 🔹 Remove Pod

```bash
podman pod rm -f demo-pod
```

---

# 🔹 Remove Volume

```bash
podman volume rm shared-vol
```

---

# 🧰 Technologies Used

<p align="center">

<img src="https://img.shields.io/badge/Podman-Pods-purple?style=for-the-badge&logo=podman">

<img src="https://img.shields.io/badge/Docker-Containers-blue?style=for-the-badge&logo=docker">

<img src="https://img.shields.io/badge/Kubernetes-Orchestration-blue?style=for-the-badge&logo=kubernetes">

<img src="https://img.shields.io/badge/OpenShift-Container_Platform-red?style=for-the-badge&logo=redhatopenshift">

<img src="https://img.shields.io/badge/Linux-System_Administration-black?style=for-the-badge&logo=linux">

<img src="https://img.shields.io/badge/DevOps-Automation-orange?style=for-the-badge&logo=azuredevops">

</p>

---

# 🎓 Conclusion

In this lab, you:

✅ Created and managed Pods using Podman  
✅ Ran multiple containers inside a Pod  
✅ Verified shared networking between containers  
✅ Implemented shared volumes  
✅ Inspected Pod networking configuration  

These concepts are foundational for Kubernetes and OpenShift environments.

---

# 🚀 Next Steps

🔹 Learn Pod Health Checks  
🔹 Explore Kubernetes Pods  
🔹 Practice Multi-Container Deployments  
🔹 Study OpenShift Pod Architecture  

---

# 📚 Additional Learning Topics

✅ Podman Health Checks  
✅ Kubernetes Networking  
✅ Container Volumes  
✅ OpenShift Development  

---

# 👨‍💻 Author

## Hafiz Muhammad Salman

📧 hafizmuhammadsalman13@gmail.com  
🌐 GitHub: https://github.com/msalman199  
📱 +92 314 3563640  

---

# ⭐ Happy Learning Pods & Container Orchestration ⭐
