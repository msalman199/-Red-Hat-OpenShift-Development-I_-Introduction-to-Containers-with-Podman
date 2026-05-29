# 🚀 Introduction to Containers with Podman

<p align="center">
  <img src="https://img.shields.io/badge/Containers-Technology-blue?style=for-the-badge&logo=docker">
  <img src="https://img.shields.io/badge/Podman-Container_Engine-purple?style=for-the-badge&logo=podman">
  <img src="https://img.shields.io/badge/Linux-Administration-black?style=for-the-badge&logo=linux">
  <img src="https://img.shields.io/badge/DevOps-Containerization-orange?style=for-the-badge&logo=kubernetes">
</p>

---

# 📘 Lab Overview

## 🎯 Objectives

By the end of this lab, you will:

✅ Understand the fundamental concepts of containerization  
✅ Learn about container isolation and portability  
✅ Gain hands-on experience running containers using Podman  

---

# 🛠️ Prerequisites

Before starting this lab, ensure you have:

🔹 Linux-based system (Ubuntu 20.04+ or Fedora recommended)  
🔹 Podman installed  
🔹 Basic command line knowledge  

---

# ⚙️ Setup

# 🔹 Install Podman

## 🐧 For Ubuntu/Debian

```bash
sudo apt-get update

sudo apt-get install -y podman
```

---

## 🐧 For Fedora/RHEL/CentOS

```bash
sudo dnf install -y podman
```

---

# 🔍 Verify Installation

```bash
podman --version
```

## ✅ Expected Output

```bash
podman version 4.x.x
```

---

# 🏗️ Task 1 — Understanding Container Basics

---

# 📦 What is a Container?

A container is a lightweight and standalone executable package that includes:

✅ Application Code  
✅ Runtime  
✅ Libraries  
✅ Dependencies  
✅ Configuration Files  

Unlike Virtual Machines, containers share the host operating system kernel, making them:

⚡ Faster  
⚡ Lightweight  
⚡ Efficient  

---

# 🔑 Key Characteristics of Containers

## 🔒 Isolation

Containers run in isolated environments that prevent interference with:

✅ Other Containers  
✅ Host Operating System  

---

## 🌍 Portability

Containers can run consistently across:

✅ Development  
✅ Testing  
✅ Production Environments  

---

# 🚀 Task 2 — Running a Simple Container with Podman

---

# 🔹 Subtask 1 — Pull a Container Image

Podman pulls container images from registries like Docker Hub.

## 📥 Pull hello-world Image

```bash
podman pull hello-world
```

---

# ✅ Expected Output

```bash
Trying to pull docker.io/library/hello-world:latest...
...
Status: Downloaded newer image for hello-world:latest
```

---

# 🔹 Subtask 2 — Run the Container

## ▶️ Execute hello-world Container

```bash
podman run hello-world
```

---

# ✅ Expected Output

```bash
Hello from Docker!

This message shows that your installation appears to be working correctly.
```

---

# 🔹 Subtask 3 — Verify Container Execution

## 📋 List All Containers

```bash
podman ps -a
```

---

# ✅ Expected Output

A table showing the hello-world container with status:

```bash
Exited
```

---

# 🔥 Task 3 — Exploring Container Isolation

---

# 🔹 Run Interactive Alpine Container

## 🐧 Launch Alpine Linux Container

```bash
podman run -it alpine sh
```

---

# ✅ Expected Output

```bash
/ #
```

You are now inside the container shell.

---

# 🔹 Verify Isolation

Inside the container, run:

```bash
cat /etc/os-release
```

---

# ✅ Expected Output

Displays Alpine Linux information, which is different from the host operating system.

---

# 🚪 Exit the Container

```bash
exit
```

---

# 🛠️ Troubleshooting Tips

---

# ❌ Permission Errors

If permission issues occur, run Podman using sudo:

```bash
sudo podman run hello-world
```

---

# 🔐 Configure Rootless Mode

```bash
podman system migrate
```

Rootless mode improves container security.

---

# 🌐 Network Issues

Ensure your system has internet connectivity to pull container images.

---

# 🐞 Enable Debug Logging

```bash
podman --log-level=debug run hello-world
```

---

# 📊 Container Commands Cheat Sheet

| Command | Description |
|---|---|
| `podman pull hello-world` | Pull container image |
| `podman run hello-world` | Run container |
| `podman ps -a` | List all containers |
| `podman images` | List images |
| `podman rm <container_id>` | Remove container |
| `podman rmi <image_id>` | Remove image |

---

# ⚡ Benefits of Containers

✅ Lightweight  
✅ Fast Startup Time  
✅ Portability  
✅ Isolation  
✅ Easy Deployment  
✅ Consistent Environment  
✅ Better Resource Utilization  

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

In this lab, you:

✅ Learned containerization fundamentals  
✅ Understood isolation and portability  
✅ Pulled and ran a container using Podman  
✅ Explored interactive containers  
✅ Verified container isolation  

Containers provide a powerful and efficient way to package and deploy applications consistently across environments.

---

# 🚀 Next Steps

🔹 Explore Podman Compose  
🔹 Learn Kubernetes  
🔹 Study OpenShift  
🔹 Build Multi-Container Applications  

---

# 📚 Additional Resources

## 📖 Podman Official Documentation

https://podman.io/docs

---

## 📖 Red Hat OpenShift Documentation

https://docs.openshift.com

---

# 👨‍💻 Author

## Hafiz Muhammad Salman

📧 hafizmuhammadsalman13@gmail.com  
🌐 GitHub: https://github.com/msalman199  
📱 +92 314 3563640  

---

# ⭐ Happy Learning Containers & DevOps ⭐
