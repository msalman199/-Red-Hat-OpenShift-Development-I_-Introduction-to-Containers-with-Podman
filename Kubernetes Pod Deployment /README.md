# ☸️ Kubernetes Pod Deployment 

<div align="center">

# 🚀 Kubernetes Pod Deployment

### Learn How to Create, Deploy, Monitor, and Troubleshoot Kubernetes Pods

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![OpenShift](https://img.shields.io/badge/OpenShift-EE0000?style=for-the-badge&logo=redhatopenshift&logoColor=white)
![Docker](https://img.shields.io/badge/Container-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![kubectl](https://img.shields.io/badge/kubectl-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)

</div>

---

# 📖 Overview

In this lab, you will learn how to deploy and manage a basic Kubernetes Pod using a YAML manifest file.

Pods are the smallest deployable units in Kubernetes and serve as the foundation for running containerized applications.

---

# 🎯 Objectives

By the end of this lab, you will be able to:

- ✅ Understand the structure of a Kubernetes Pod YAML manifest
- ✅ Deploy a Pod using Kubernetes or OpenShift
- ✅ Verify Pod deployment status
- ✅ Inspect Pod details and events
- ✅ View Pod logs for troubleshooting
- ✅ Access applications running inside Pods

---

# 📋 Prerequisites

Before starting this lab, ensure you have:

| Requirement | Description |
|------------|------------|
| ☸️ Kubernetes Cluster | Minikube, Kind, K3s, EKS, AKS, GKE, etc. |
| 🔴 OpenShift Cluster | Optional |
| 🖥️ kubectl / oc CLI | Installed and configured |
| 📝 YAML Knowledge | Basic understanding |
| 🐳 Container Knowledge | Recommended |

---

# 🏗️ Kubernetes Pod Architecture

```text
┌─────────────────────────────┐
│      Kubernetes Cluster     │
└──────────────┬──────────────┘
               │
               ▼

┌─────────────────────────────┐
│         nginx-pod           │
├─────────────────────────────┤
│  nginx-container            │
│  Image: nginx:latest        │
│  Port: 80                   │
└─────────────────────────────┘
```

---

# 🛠️ Task 1: Write a Pod YAML Manifest

In this task, you will create a YAML file that defines a simple Nginx Pod.

---

## 📌 Subtask 1.1: Create a Pod Manifest File

Open your favorite text editor:

- 📝 VS Code
- 📝 Nano
- 📝 Vim

Create a file named:

```text
simple-pod.yaml
```

Add the following content:

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod
  labels:
    app: nginx

spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

---

## 📚 YAML Manifest Explanation

### 🔹 apiVersion

Specifies the Kubernetes API version.

```yaml
apiVersion: v1
```

---

### 🔹 kind

Defines the Kubernetes resource type.

```yaml
kind: Pod
```

---

### 🔹 metadata

Contains resource information such as:

- Name
- Labels
- Annotations

```yaml
metadata:
  name: nginx-pod
```

---

### 🔹 spec

Defines how the Pod should run.

```yaml
spec:
```

---

### 🔹 containers

Lists the containers inside the Pod.

```yaml
containers:
```

---

### 🔹 image

Container image to run.

```yaml
image: nginx:latest
```

---

### 🔹 containerPort

Exposes a port inside the container.

```yaml
containerPort: 80
```

---

## 🎯 Expected Outcome

You should have a valid Kubernetes YAML manifest that defines an Nginx Pod.

---

# 🚀 Task 2: Deploy the Pod

Now deploy the Pod to your Kubernetes cluster.

---

## ▶️ Subtask 2.1: Apply the YAML File

Run:

```bash
kubectl apply -f simple-pod.yaml
```

### Expected Output

```bash
pod/nginx-pod created
```

---

## 🔍 Subtask 2.2: Verify Pod Deployment

Check Pod status:

```bash
kubectl get pods
```

### Expected Output

```bash
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          10s
```

---

## 🎯 Understanding Pod Status

| Status | Meaning |
|----------|----------|
| 🟢 Running | Pod is healthy |
| 🟡 Pending | Waiting for resources |
| 🔴 CrashLoopBackOff | Application repeatedly crashing |
| ⚫ Failed | Pod execution failed |
| 🔵 Completed | Job finished successfully |

---

# 🚨 Troubleshooting Tips

## ⚠️ Pod Stuck in Pending State

Check Pod details:

```bash
kubectl describe pod nginx-pod
```

Possible causes:

- Insufficient CPU
- Insufficient Memory
- Node unavailable
- Image pull issues

---

## ⚠️ Pod in CrashLoopBackOff

Check logs:

```bash
kubectl logs nginx-pod
```

Look for:

- Application errors
- Missing configuration
- Invalid startup commands

---

# 🔎 Task 3: Inspect Pod Status and Logs

In this task, you will inspect the running Pod.

---

## 📌 Subtask 3.1: Check Pod Details

Display detailed information:

```bash
kubectl describe pod nginx-pod
```

---

### Expected Information

You will see:

- Pod Name
- Namespace
- Labels
- Node Assignment
- Container Details
- Pod Events
- IP Address
- Status Information

---

## 📌 Subtask 3.2: View Pod Logs

Retrieve logs from the container:

```bash
kubectl logs nginx-pod
```

### Expected Output

```text
Nginx access logs
Nginx error logs
```

---

## 📌 Subtask 3.3: Access the Pod (Optional)

Forward the Pod port to your local machine:

```bash
kubectl port-forward nginx-pod 8080:80
```

### Expected Output

```bash
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
```

---

## 🌐 Access Nginx

Open your browser:

```text
http://localhost:8080
```

### Expected Result

🎉 Nginx Welcome Page

---

# 🧪 Useful kubectl Commands

## View Pods

```bash
kubectl get pods
```

---

## View Detailed Pod Information

```bash
kubectl describe pod nginx-pod
```

---

## View Logs

```bash
kubectl logs nginx-pod
```

---

## Delete Pod

```bash
kubectl delete pod nginx-pod
```

---

## View Cluster Nodes

```bash
kubectl get nodes
```

---

## View All Resources

```bash
kubectl get all
```

---

# 🏗️ Deployment Workflow

```text
Write YAML Manifest
          │
          ▼
kubectl apply -f simple-pod.yaml
          │
          ▼
 Kubernetes API Server
          │
          ▼
 Scheduler Selects Node
          │
          ▼
 Pod Created
          │
          ▼
 Container Starts
          │
          ▼
 Verify Pod Status
          │
          ▼
 Access Application
```

---

# 🎉 Conclusion

In this lab, you successfully:

✅ Created a Kubernetes Pod YAML manifest

✅ Defined an Nginx container inside a Pod

✅ Deployed the Pod using kubectl

✅ Verified Pod status

✅ Inspected Pod details

✅ Viewed container logs

✅ Accessed the running application

These are foundational Kubernetes skills required for deploying and managing containerized workloads.

---

# 🚀 Next Steps

Continue your Kubernetes journey by learning:

- 🔹 Multi-Container Pods
- 🔹 ReplicaSets
- 🔹 Deployments
- 🔹 Services
- 🔹 ConfigMaps
- 🔹 Secrets
- 🔹 Persistent Volumes
- 🔹 Health Checks & Probes
- 🔹 Pod Lifecycle Management

---

# 📚 Additional Resources

## Kubernetes Documentation

https://kubernetes.io/docs/concepts/workloads/pods/

---

## OpenShift Pod Management

https://docs.openshift.com/

---

## kubectl Cheat Sheet

https://kubernetes.io/docs/reference/kubectl/cheatsheet/

---

<div align="center">
### ☸️ Welcome to the Kubernetes World!

**Happy Learning & Happy Deploying! 🚀**

</div>
