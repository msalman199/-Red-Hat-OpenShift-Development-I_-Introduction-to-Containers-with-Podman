# 🚀 Docker Compose with Dependencies 

<div align="center">

# 🐳 Docker Compose Dependencies & Scaling

### Learn Service Dependencies, Scaling, and Inter-Service Communication

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Docker Compose](https://img.shields.io/badge/Docker_Compose-1D63ED?style=for-the-badge&logo=docker&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)

</div>

---

# 📖 Overview

This lab demonstrates how to manage service dependencies using Docker Compose, scale application services, and test communication between containers.

You will learn how to:

- ✅ Use the `depends_on` directive
- ✅ Scale services with Docker Compose
- ✅ Verify service startup order
- ✅ Test container-to-container communication
- ✅ Build and deploy a Flask application connected to Redis

---

# 🎯 Objectives

By the end of this lab, you will be able to:

- 🔹 Manage service dependencies using `depends_on`
- 🔹 Scale services using Docker Compose
- 🔹 Verify service startup order
- 🔹 Test connectivity between containers
- 🔹 Deploy a multi-service application using Flask and Redis

---

# 📋 Prerequisites

Before starting this lab, ensure you have:

| Requirement | Description |
|------------|------------|
| 🐳 Docker | Version 20.10.0 or later |
| 📦 Docker Compose | Version 2.0.0 or later |
| 📘 Docker Knowledge | Basic understanding of containers |
| 💻 Terminal Access | Linux, macOS, or Windows |

---

# ⚙️ Lab Setup

## 📁 Create Project Directory

```bash
mkdir compose-dependencies-lab
cd compose-dependencies-lab
```

---

# 🛠️ Task 1: Using depends_on Directive

## 📌 Subtask 1.1: Create a Basic Compose File

Create a file named:

```text
docker-compose.yml
```

Add the following content:

```yaml
version: '3.8'

services:
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"

  webapp:
    image: nginx:alpine
    ports:
      - "8080:80"
    depends_on:
      - redis
```

---

## 📌 Subtask 1.2: Understand the depends_on Directive

### 🔍 What does depends_on do?

The `depends_on` directive controls startup order.

```yaml
depends_on:
  - redis
```

### Key Points

✅ Redis starts before WebApp

✅ Ensures dependency order

⚠️ Does NOT guarantee Redis is fully ready

⚠️ Only ensures Redis container has started

---

## ▶️ Subtask 1.3: Start the Services

```bash
docker-compose up -d
```

---

## 🔍 Verify Running Services

```bash
docker-compose ps
```

### Expected Outcome

- ✅ Redis container starts first
- ✅ Nginx container starts after Redis
- ✅ Both containers are running

---

# 🚀 Task 2: Scale Service Replicas

## 📌 Subtask 2.1: Modify Compose File

Update the compose file:

```yaml
version: '3.8'

services:
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"

  webapp:
    image: nginx:alpine
    ports:
      - "8080-8082:80"
    depends_on:
      - redis
    deploy:
      replicas: 3
```

---

## ▶️ Subtask 2.2: Start Scaled Services

```bash
docker-compose up -d --scale webapp=3
```

---

## 🔍 Subtask 2.3: Verify Scaling

Check running containers:

```bash
docker-compose ps
```

Check hostname:

```bash
docker-compose exec webapp hostname
```

---

### Expected Outcome

✅ One Redis instance

✅ Three Nginx containers

✅ Unique hostname for each Nginx instance

---

# 🌐 Task 3: Test Inter-Service Connectivity

## 📌 Subtask 3.1: Create Flask Application

Create:

```text
app.py
```

Add:

```python
from flask import Flask
import redis
import os

app = Flask(__name__)

redis_host = os.environ.get('REDIS_HOST', 'redis')

redis_client = redis.Redis(
    host=redis_host,
    port=6379
)

@app.route('/')
def hello():
    count = redis_client.incr('hits')
    return f'Hello World! This page has been viewed {count} times.\n'

if __name__ == '__main__':
    app.run(
        host='0.0.0.0',
        port=5000
    )
```

---

## 📌 Create Dockerfile

Create:

```text
Dockerfile
```

Add:

```dockerfile
FROM python:3.9-alpine

WORKDIR /app

COPY . .

RUN pip install flask redis

CMD ["python", "app.py"]
```

---

## 📌 Subtask 3.2: Update Compose File

```yaml
version: '3.8'

services:

  redis:
    image: redis:alpine

  webapp:
    build: .
    ports:
      - "5000:5000"

    environment:
      - REDIS_HOST=redis

    depends_on:
      - redis
```

---

## ▶️ Subtask 3.3: Build and Start

```bash
docker-compose up -d
```

---

## 🌍 Test the Application

```bash
curl http://localhost:5000
```

### Expected Output

```text
Hello World! This page has been viewed 1 times.
```

Subsequent requests:

```text
Hello World! This page has been viewed 2 times.
```

```text
Hello World! This page has been viewed 3 times.
```

---

## 🔍 What Happens?

1️⃣ Flask receives request

2️⃣ Flask connects to Redis

3️⃣ Redis increments counter

4️⃣ Flask displays updated count

5️⃣ Response returned to browser

---

# 🏗️ Architecture Diagram

```text
                    ┌───────────────┐
                    │    Client     │
                    └───────┬───────┘
                            │
                            ▼

                 ┌─────────────────────┐
                 │  Flask Web Service  │
                 │      Port 5000      │
                 └──────────┬──────────┘
                            │
                            │ Redis Network
                            ▼

                  ┌──────────────────┐
                  │      Redis       │
                  │    Port 6379     │
                  └──────────────────┘
```

---

# 🚨 Troubleshooting Tips

## ⚠️ Services Fail to Start

Check logs:

```bash
docker-compose logs
```

Verify:

- Docker is running
- Ports are available
- System resources are sufficient

---

## ⚠️ Scaling Issues

Ensure:

```bash
docker-compose version
```

Using:

```text
Docker Compose v2+
```

Check:

- Port conflicts
- Available resources

---

## ⚠️ Connectivity Issues

Verify:

- Service names match
- Redis container is running
- Redis is ready to accept connections

Inspect network:

```bash
docker network ls
```

---

# 🎉 Conclusion

In this lab, you learned:

✅ How to use `depends_on`

✅ Service startup ordering

✅ Scaling services with Docker Compose

✅ Container-to-container communication

✅ Building a Flask application

✅ Connecting Flask with Redis

These concepts are fundamental for deploying production-grade multi-service applications.

---

# 🧹 Cleanup

Remove all containers, networks, and resources:

```bash
docker-compose down
```

Verify cleanup:

```bash
docker ps -a
```

---

# 📚 Additional Resources

### Docker Compose Documentation

https://docs.docker.com/compose/

### Redis Official Image

https://hub.docker.com/_/redis

### Flask Documentation

https://flask.palletsprojects.com/

---

# 🚀 Next Steps

- 🔹 Learn Docker Compose Networking
- 🔹 Explore Named Volumes
- 🔹 Deploy Multi-Tier Applications
- 🔹 Use Health Checks
- 🔹 Learn Docker Swarm
- 🔹 Explore Kubernetes Fundamentals
