# 🚀 Exploring Podman CLI

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

✅ Understand and use basic Podman commands  
✅ Manage container lifecycle  
✅ Inspect container details  
✅ List running and stopped containers  

---

# 🛠️ Prerequisites

Before starting this lab, ensure you have:

🔹 Linux System (Fedora, CentOS, RHEL, Ubuntu)  
🔹 Podman Installed  
🔹 Basic Linux Command Line Knowledge  
🔹 Internet Access  

---

# ⚙️ Setup Requirements

# 🔹 Install Podman

## 🐧 For Fedora/CentOS/RHEL

```bash
sudo dnf install podman
```

---

## 🐧 For Ubuntu/Debian

```bash
sudo apt-get update && sudo apt-get install podman
```

---

# 🔍 Verify Installation

```bash
podman --version
```

---

# ✅ Expected Output

```bash
podman version 4.x.x
```

---

# 🏗️ Task 1 — Listing Containers

## 🎯 Objective

Learn how to list running and stopped containers.

---

# 🔹 Step 1 — List Running Containers

```bash
podman ps
```

---

# ✅ Expected Output

If no containers are running, output will be empty.

---

# 🔹 Step 2 — List All Containers

```bash
podman ps -a
```

---

# ✅ Expected Output

Displays all containers including stopped containers.

---

# 🛠️ Troubleshooting Tip

If permission errors occur:

```bash
sudo podman ps
```

Or configure rootless Podman correctly.

---

# 🚀 Task 2 — Running a Container

## 🎯 Objective

Learn how to run containers using Podman.

---

# 🔹 Step 1 — Run Alpine Linux Container

```bash
podman run -it --name my_alpine alpine sh
```

---

# 📖 Command Explanation

| Option | Description |
|---|---|
| `-it` | Interactive terminal |
| `--name` | Assign container name |
| `alpine` | Container image |
| `sh` | Shell command |

---

# ✅ Expected Outcome

You will enter the Alpine Linux shell.

```bash
/ #
```

---

# 🚪 Exit the Container

```bash
exit
```

---

# 🔹 Step 2 — Verify Running Container

```bash
podman ps
```

---

# ✅ Expected Output

The container `my_alpine` should appear in the list.

---

# 🛑 Task 3 — Stopping a Container

## 🎯 Objective

Learn how to stop running containers.

---

# 🔹 Step 1 — Stop Container

```bash
podman stop my_alpine
```

---

# ✅ Verify Status

```bash
podman ps -a
```

Expected Status:

```bash
Exited
```

---

# 🔄 Task 4 — Restarting a Container

## 🎯 Objective

Learn how to restart stopped containers.

---

# 🔹 Step 1 — Restart Container

```bash
podman restart my_alpine
```

---

# ✅ Verify Restart

```bash
podman ps
```

Expected Status:

```bash
Up
```

---

# 🗑️ Task 5 — Removing a Container

## 🎯 Objective

Learn how to remove containers.

---

# 🔹 Step 1 — Stop the Container

```bash
podman stop my_alpine
```

---

# 🔹 Step 2 — Remove the Container

```bash
podman rm my_alpine
```

---

# ✅ Verify Removal

```bash
podman ps -a
```

The container should no longer appear.

---

# 🔍 Task 6 — Inspecting Container Details

## 🎯 Objective

Learn how to inspect container metadata.

---

# 🔹 Step 1 — Run Nginx Container

```bash
podman run -d --name nginx_container nginx
```

---

# 📖 Command Explanation

| Option | Description |
|---|---|
| `-d` | Detached Mode |
| `--name` | Assign Name |
| `nginx` | Official Nginx Image |

---

# 🔹 Step 2 — Inspect Container

```bash
podman inspect nginx_container
```

---

# ✅ Expected Output

Detailed JSON output showing:

✅ Container Configuration  
✅ Network Settings  
✅ Container State  
✅ Mounted Volumes  

---

# 🔹 Step 3 — Clean Up

```bash
podman stop nginx_container && podman rm nginx_container
```

---

# 📊 Common Podman Commands Cheat Sheet

| Command | Description |
|---|---|
| `podman ps` | List running containers |
| `podman ps -a` | List all containers |
| `podman run` | Run container |
| `podman stop` | Stop container |
| `podman restart` | Restart container |
| `podman rm` | Remove container |
| `podman inspect` | Inspect container |
| `podman images` | List images |

---

# ⚡ Benefits of Podman

✅ Daemonless Architecture  
✅ Rootless Containers  
✅ Better Security  
✅ OCI Compatible  
✅ Lightweight  
✅ Docker Compatible CLI  

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

✅ Listing containers (`podman ps`, `podman ps -a`)  
✅ Running containers (`podman run`)  
✅ Stopping containers (`podman stop`)  
✅ Restarting containers (`podman restart`)  
✅ Removing containers (`podman rm`)  
✅ Inspecting containers (`podman inspect`)  

These skills are foundational for container management and OpenShift environments.

---

# 🚀 Next Steps

🔹 Explore Podman Networking  
🔹 Learn Volume Management  
🔹 Run Multi-Container Applications  
🔹 Prepare for OpenShift Certification  

---

# 📚 Additional Learning Topics

✅ Podman Compose  
✅ Kubernetes Basics  
✅ OpenShift Development  
✅ Container Security  

---

# 👨‍💻 Author

## Hafiz Muhammad Salman

📧 hafizmuhammadsalman13@gmail.com  
🌐 GitHub: https://github.com/msalman199  
📱 +92 314 3563640  

---

# ⭐ Happy Learning Containers & DevOps ⭐
