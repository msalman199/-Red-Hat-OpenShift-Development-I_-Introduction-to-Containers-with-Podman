# 🚀 Podman Compose Basics 

<div align="center">

# 🐳 Podman Compose Basics

### Deploy & Manage Multi-Container Applications with Podman Compose

![Podman](https://img.shields.io/badge/Podman-892CA0?style=for-the-badge&logo=podman&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Containers](https://img.shields.io/badge/Containers-2496ED?style=for-the-badge&logo=docker&logoColor=white)

</div>

---

# 📖 Overview

This lab introduces **Podman Compose**, a tool used to define and manage multi-container applications using a YAML configuration file.

## 🎯 Objectives

By the end of this lab, you will be able to:

- ✅ Define multi-container applications using Podman Compose
- ✅ Configure services, ports, and volumes in a `podman-compose.yml` file
- ✅ Start and stop multi-container applications with Podman Compose
- ✅ Test and verify running services
- ✅ Clean up containers and networks

---

# 📋 Prerequisites

Before starting this lab, ensure you have:

| Requirement | Description |
|------------|------------|
| 🐳 Podman | Version 3.0 or later |
| 📦 Podman Compose | Installed via pip or package manager |
| 📘 YAML Knowledge | Basic understanding of YAML syntax |
| 🖥 Linux Environment | Linux system or WSL2 on Windows |

---

# ⚙️ Lab Setup

## 🔍 Verify Podman Installation

```bash
podman --version
```

### Expected Output

```bash
podman version 3.x.x
```

---

## 🔍 Verify Podman Compose Installation

```bash
podman-compose --version
```

### Expected Output

```bash
podman-compose version x.x.x
```

---

# 🛠️ Task 1: Create a Simple podman-compose.yml File

## 📌 Subtask 1.1: Understand the Basic Structure

A `podman-compose.yml` file defines:

- 🐳 Services
- 🌐 Networks
- 💾 Volumes

Podman Compose uses the same syntax as Docker Compose Version 3.

---

## 📌 Subtask 1.2: Create Project Directory

```bash
mkdir compose-lab
cd compose-lab
touch podman-compose.yml
```

---

## 📝 Create the Compose File

Edit `podman-compose.yml` and add:

```yaml
version: '3.8'

services:
  web:
    image: docker.io/nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html

  db:
    image: docker.io/postgres:13
    environment:
      POSTGRES_PASSWORD: example
```

---

## 📚 Key Concepts

### 🐳 Services

Defines the containers in your application.

```yaml
services:
```

### 🌐 Ports

Maps host ports to container ports.

```yaml
ports:
  - "8080:80"
```

### 💾 Volumes

Persist data and share files between host and container.

```yaml
volumes:
  - ./html:/usr/share/nginx/html
```

### 🔐 Environment Variables

Pass configuration values into containers.

```yaml
environment:
  POSTGRES_PASSWORD: example
```

---

# 🚀 Task 2: Start the Multi-Container Application

## ▶️ Subtask 2.1: Launch the Application

```bash
podman-compose up -d
```

### Expected Output

```bash
Creating network "compose-lab_default" with the default driver
Creating volume "compose-lab_db_data" with default driver
Pulling image docker.io/nginx:alpine...
Pulling image docker.io/postgres:13...
Creating compose-lab_web_1 ... done
Creating compose-lab_db_1  ... done
```

---

## 🔍 Subtask 2.2: Verify Running Containers

```bash
podman ps
```

### Expected Output

```bash
CONTAINER ID  IMAGE                           COMMAND
abc123        docker.io/nginx:alpine
def456        docker.io/postgres:13
```

---

# 🧪 Task 3: Test the Application

## 🌐 Subtask 3.1: Access the Web Service

Create an HTML directory:

```bash
mkdir html
```

Create a sample webpage:

```bash
echo "<h1>Hello from Podman Compose!</h1>" > html/index.html
```

Open your browser:

```text
http://localhost:8080
```

Or use:

```bash
curl http://localhost:8080
```

### Expected Output

```html
<h1>Hello from Podman Compose!</h1>
```

---

## 🗄️ Subtask 3.2: Verify Database Service

Connect to PostgreSQL:

```bash
podman exec -it compose-lab_db_1 psql -U postgres
```

Run:

```sql
SELECT version();
```

Exit PostgreSQL:

```sql
\q
```

### Expected Output

```text
PostgreSQL 13.x on x86_64-pc-linux-gnu...
```

---

# 🧹 Task 4: Stop and Clean Up

## ⏹️ Subtask 4.1: Stop the Application

```bash
podman-compose down
```

### Expected Output

```bash
Stopping compose-lab_web_1 ... done
Stopping compose-lab_db_1  ... done
Removing compose-lab_web_1 ... done
Removing compose-lab_db_1  ... done
Removing network compose-lab_default
```

---

## 🔍 Subtask 4.2: Verify Cleanup

```bash
podman ps -a
```

### Expected Output

```text
No containers should be listed
```

---

# 🚨 Troubleshooting Tips

## ⚠️ Port Conflicts

If port 8080 is already in use:

```yaml
ports:
  - "8081:80"
```

---

## ⚠️ SELinux Permission Issues

Use the `:Z` suffix:

```yaml
volumes:
  - ./html:/usr/share/nginx/html:Z
```

---

## ⚠️ Image Pull Errors

Verify:

- 🌐 Internet connection
- 🐳 Correct image names
- 🔐 Registry access permissions

---

# 🏗️ Architecture Diagram

```text
┌─────────────────────┐
│     Host System     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Podman Compose     │
└───────┬─────┬───────┘
        │     │
        ▼     ▼

┌────────────┐   ┌─────────────┐
│ Nginx Web  │   │ PostgreSQL  │
│ Container  │   │ Container   │
└────────────┘   └─────────────┘
```

---

# 🎉 Conclusion

In this lab, you have:

- ✅ Created a `podman-compose.yml` file defining multiple services
- ✅ Configured port mappings and volumes
- ✅ Successfully started and tested a multi-container application
- ✅ Verified web and database services
- ✅ Learned to stop and clean up the application

This foundational knowledge enables you to deploy more complex containerized applications using Podman Compose.

---

# 🚀 Next Steps

### Continue Your Learning Journey

- 🔹 Explore advanced Podman Compose configurations
- 🔹 Learn Podman networking concepts
- 🔹 Practice with named volumes and tmpfs
- 🔹 Build multi-tier applications
- 🔹 Integrate Podman Compose into CI/CD pipelines
- 🔹 Deploy production-ready container environments
