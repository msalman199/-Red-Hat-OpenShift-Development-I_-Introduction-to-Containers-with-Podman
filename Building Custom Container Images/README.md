# 🚀 Building Custom Container Images with Podman

<p align="center">
  <img src="https://img.shields.io/badge/Podman-Container_Engine-purple?style=for-the-badge&logo=podman">
  <img src="https://img.shields.io/badge/Containers-Custom_Images-blue?style=for-the-badge&logo=docker">
  <img src="https://img.shields.io/badge/Linux-System_Administration-black?style=for-the-badge&logo=linux">
  <img src="https://img.shields.io/badge/DevOps-Containerization-orange?style=for-the-badge&logo=kubernetes">
</p>

---

# 📘 Building Custom Container Images

## 🎯 Objectives

By the end of this lab, you will be able to:

✅ Understand the structure of a Containerfile (Dockerfile alternative for Podman)  
✅ Use key Containerfile instructions (`FROM`, `RUN`, `COPY`, `ENV`)  
✅ Build and tag a custom container image using Podman  
✅ Run and verify containerized applications  

---

# 🛠️ Prerequisites

Before starting this lab, ensure you have:

🔹 Linux-based environment (Fedora, Ubuntu, RHEL, etc.)  
🔹 Podman installed  
🔹 Basic command-line knowledge  

---

# ⚙️ Verify Podman Installation

Run the following command:

```bash
podman --version
```

---

# ✅ Expected Output

```bash
podman version 4.x.x
```

---

# 🔹 Install Podman (If Needed)

## 🐧 Fedora / RHEL

```bash
sudo dnf install podman
```

## 🐧 Ubuntu / Debian

```bash
sudo apt-get install podman
```

---

# 🏗️ Lab Tasks

# 🚀 Task 1 — Create a Simple Containerfile

## 🎯 Objective

Create a custom Nginx-based image with a custom welcome page.

---

# 🔹 Step 1 — Create Project Directory

```bash
mkdir custom-nginx && cd custom-nginx
```

---

# 🔹 Step 2 — Create Custom HTML File

Create an `index.html` file:

```bash
echo "<h1>Welcome to My Custom Nginx Container!</h1>" > index.html
```

---

# 🔹 Step 3 — Create the Containerfile

Create a file named `Containerfile`:

```Dockerfile
# Use the official Nginx base image
FROM docker.io/nginx:alpine

# Set an environment variable
ENV AUTHOR="OpenShift Developer"

# Copy the custom HTML file to the Nginx web root
COPY index.html /usr/share/nginx/html

# Run a command to print a message
RUN echo "Container built by $AUTHOR" > /build-info.txt
```

---

# 📖 Key Containerfile Instructions

| Instruction | Description |
|---|---|
| `FROM` | Specifies the base image |
| `ENV` | Sets environment variables |
| `COPY` | Copies files into image |
| `RUN` | Executes commands during build |

---

# 🚀 Task 2 — Build and Tag the Image

## 🎯 Objective

Build and manage a custom container image.

---

# 🔹 Step 1 — Build the Image

```bash
podman build -t my-custom-nginx .
```

---

# 📖 Command Explanation

| Option | Description |
|---|---|
| `build` | Build container image |
| `-t` | Assign image tag |
| `.` | Current directory as build context |

---

# ✅ Expected Output

```bash
STEP 1/4: FROM docker.io/nginx:alpine
STEP 2/4: ENV AUTHOR="OpenShift Developer"
STEP 3/4: COPY index.html /usr/share/nginx/html
STEP 4/4: RUN echo "Container built by $AUTHOR" > /build-info.txt
COMMIT my-custom-nginx
```

---

# 🔹 Step 2 — Verify Built Images

```bash
podman images
```

---

# ✅ Expected Output

```bash
REPOSITORY                  TAG       IMAGE ID       CREATED         SIZE
localhost/my-custom-nginx  latest    xxxxxxxxxxxx   1 minute ago    25.4 MB
```

---

# 🔹 Step 3 — Run the Container

```bash
podman run -d -p 8080:80 my-custom-nginx
```

---

# 📖 Command Explanation

| Option | Description |
|---|---|
| `-d` | Detached mode |
| `-p 8080:80` | Port mapping |

---

# 🔹 Verify Running Container

```bash
podman ps
```

---

# 🌐 Step 4 — Test the Web Server

Open your browser and visit:

```text
http://localhost:8080
```

---

# ✅ Expected Output

```html
Welcome to My Custom Nginx Container!
```

---

# 📊 Common Podman Commands

| Command | Description |
|---|---|
| `podman build` | Build container image |
| `podman images` | List images |
| `podman run` | Run container |
| `podman ps` | List running containers |
| `podman logs` | View logs |

---

# 🔑 Key Concepts

## 📦 Containerfile

A file used to define how a container image is built.

---

## 🏷️ Image Tagging

Tags help organize versions.

Example:

```bash
my-custom-nginx:latest
```

---

## 🌐 Port Mapping

Maps host ports to container ports.

Example:

```bash
-p 8080:80
```

---

# ⚡ Benefits of Custom Container Images

✅ Portable applications  
✅ Faster deployments  
✅ Consistent environments  
✅ Easy scaling  
✅ DevOps automation  

---

# 🛠️ Troubleshooting Tips

# ❌ Build Fails

Check for:

✅ Typos in Containerfile  
✅ Missing files  
✅ Invalid instructions  

---

# ❌ Container Not Starting

Check container status:

```bash
podman ps -a
```

---

# ❌ Debug Container Logs

```bash
podman logs <container_id>
```

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

✅ Create a Containerfile  
✅ Use `FROM`, `RUN`, `COPY`, and `ENV` instructions  
✅ Build and tag custom images  
✅ Run and verify containers  

These are essential skills for cloud-native and DevOps environments.

---

# 🚀 Next Steps

🔹 Learn additional instructions (`WORKDIR`, `CMD`, `EXPOSE`)  
🔹 Push images to Quay.io or Docker Hub  
🔹 Explore multi-stage builds  
🔹 Build production-ready containers  

---

# 📚 Additional Learning Topics

✅ Dockerfile Best Practices  
✅ Container Security  
✅ Image Optimization  
✅ Kubernetes Deployments  

---

# 👨‍💻 Author

## Hafiz Muhammad Salman

📧 hafizmuhammadsalman13@gmail.com  
🌐 GitHub: https://github.com/msalman199  
📱 +92 314 3563640  

---

# ⭐ Happy Learning Container Image Building ⭐
