
# 🚀 Managing Container Images with Podman

<p align="center">
  <img src="https://img.shields.io/badge/Podman-Container_Engine-purple?style=for-the-badge&logo=podman">
  <img src="https://img.shields.io/badge/Containers-Images-blue?style=for-the-badge&logo=docker">
  <img src="https://img.shields.io/badge/Linux-System_Administration-black?style=for-the-badge&logo=linux">
  <img src="https://img.shields.io/badge/DevOps-Containerization-orange?style=for-the-badge&logo=kubernetes">
</p>

---

# 📘 Lab Overview

## 🎯 Objectives

By the end of this lab, you will be able to:

✅ Search for container images on Docker Hub  
✅ Pull container images to your local system  
✅ Inspect image metadata and layers  
✅ Remove container images from your system  

---

# 🛠️ Prerequisites

Before starting this lab, ensure you have:

🔹 Linux-based System (Ubuntu 20.04/22.04 Recommended)  
🔹 Podman Installed (Version 3.0+)  
🔹 Internet Connectivity  
🔹 Basic Command-Line Knowledge  

---

# ⚙️ Setup Instructions

# 🔹 Install Podman

```bash
sudo apt-get update
sudo apt-get install -y podman
```

---

# 🔹 Verify Installation

```bash
podman --version
```

---

# ✅ Expected Output

```bash
podman version 3.x.x
```

---

# 🔍 Task 1 — Search for Images on Docker Hub

## 🎯 Objective

Learn how to search for container images using Podman.

---

# 🔹 Subtask 1.1 — Search for Ubuntu Image

```bash
podman search ubuntu
```

---

# ✅ Expected Output

Displays available Ubuntu images with:

✅ Image Name  
✅ Description  
✅ Stars  
✅ Official Status  

---

# 🔹 Filter Official Images

```bash
podman search --filter=is-official=true ubuntu
```

---

# 📖 Key Concept

| Feature | Description |
|---|---|
| `podman search` | Search container registry |
| `--filter` | Apply search filters |
| `is-official=true` | Show official images only |

---

# 🛠️ Troubleshooting

If Docker Hub rate limit occurs:

```bash
podman login docker.io
```

---

# 📥 Task 2 — Pull Container Images

## 🎯 Objective

Learn how to download container images locally.

---

# 🔹 Subtask 2.1 — Pull Latest Ubuntu Image

```bash
podman pull docker.io/library/ubuntu:latest
```

---

# 🔹 Verify Downloaded Images

```bash
podman images
```

---

# ✅ Expected Output

Displays:

✅ Repository  
✅ Tag  
✅ Image ID  
✅ Size  

---

# 🔹 Subtask 2.2 — Pull Specific Ubuntu Version

```bash
podman pull docker.io/library/ubuntu:20.04
```

---

# 🔹 Verify Images Again

```bash
podman images
```

---

# 📖 Key Concept

## 🏷️ Image Tags

Tags help manage:

✅ Different Versions  
✅ Stable Releases  
✅ Testing Environments  
✅ Rollbacks  

Example:

| Tag | Meaning |
|---|---|
| `latest` | Latest Version |
| `20.04` | Ubuntu 20.04 Release |

---

# 🔎 Task 3 — Inspect Image Metadata

## 🎯 Objective

Learn how to inspect image details and metadata.

---

# 🔹 Subtask 3.1 — Inspect Ubuntu Image

```bash
podman inspect docker.io/library/ubuntu:latest
```

---

# ✅ Expected Output

JSON-formatted metadata including:

✅ Architecture  
✅ Environment Variables  
✅ Layers  
✅ Entrypoint  
✅ Labels  

---

# 🔹 Subtask 3.2 — Extract Environment Variables

```bash
podman inspect --format "{{.Config.Env}}" docker.io/library/ubuntu:latest
```

---

# 📖 Key Concept

Image inspection helps:

✅ Understand image configuration  
✅ Verify runtime settings  
✅ Analyze image layers  
✅ Troubleshoot deployments  

---

# 🗑️ Task 4 — Remove Container Images

## 🎯 Objective

Learn how to remove images from the local system.

---

# 🔹 Subtask 4.1 — Remove Ubuntu 20.04 Image

```bash
podman rmi docker.io/library/ubuntu:20.04
```

---

# 🔹 Verify Removal

```bash
podman images
```

---

# 🔹 Subtask 4.2 — Remove Unused Images

```bash
podman image prune -a
```

---

# 📖 Key Concept

| Command | Description |
|---|---|
| `podman rmi` | Remove Image |
| `podman image prune -a` | Remove Unused Images |

---

# 🛠️ Troubleshooting

If image is being used by a container:

```bash
podman rmi -f IMAGE_ID
```

---

# 📊 Common Podman Image Commands

| Command | Description |
|---|---|
| `podman search` | Search Images |
| `podman pull` | Download Image |
| `podman images` | List Images |
| `podman inspect` | Inspect Image |
| `podman rmi` | Remove Image |
| `podman image prune` | Clean Unused Images |

---

# ⚡ Benefits of Managing Images Properly

✅ Faster Deployments  
✅ Better Version Control  
✅ Reduced Disk Usage  
✅ Easier Troubleshooting  
✅ Improved Security  

---

# 🧰 Technologies Used

<p align="center">

<img src="https://img.shields.io/badge/Podman-Container_Engine-purple?style=for-the-badge&logo=podman">

<img src="https://img.shields.io/badge/Docker-Container_Images-blue?style=for-the-badge&logo=docker">

<img src="https://img.shields.io/badge/Linux-System_Administration-black?style=for-the-badge&logo=linux">

<img src="https://img.shields.io/badge/Kubernetes-Orchestration-blue?style=for-the-badge&logo=kubernetes">

<img src="https://img.shields.io/badge/OpenShift-Container_Platform-red?style=for-the-badge&logo=redhatopenshift">

<img src="https://img.shields.io/badge/DevOps-Automation-orange?style=for-the-badge&logo=azuredevops">

</p>

---

# 🎓 Conclusion

In this lab, you learned how to:

✅ Search for images on Docker Hub  
✅ Pull container images locally  
✅ Inspect image metadata and layers  
✅ Remove unused images from the system  

These are essential skills for managing containerized applications efficiently.

---

# 🚀 Next Steps

🔹 Practice with Quay.io Registry  
🔹 Explore Red Hat Registries  
🔹 Learn Image Tagging Strategies  
🔹 Build Custom Images with Dockerfiles  

---

# 📚 Additional Learning Topics

✅ Multi-Stage Builds  
✅ Container Security Scanning  
✅ Image Optimization  
✅ Kubernetes Image Management  

---

# 👨‍💻 Author

## Hafiz Muhammad Salman

📧 hafizmuhammadsalman13@gmail.com  
🌐 GitHub: https://github.com/msalman199  
📱 +92 314 3563640  

---

# ⭐ Happy Learning Container Image Management ⭐
