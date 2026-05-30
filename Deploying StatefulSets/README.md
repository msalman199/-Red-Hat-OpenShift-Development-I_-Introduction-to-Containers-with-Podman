# ☸️ Deploying StatefulSets

<div align="center">

# 🚀 Kubernetes StatefulSets Deployment

### Learn Persistent Storage, Stable Networking, and Stateful Workloads

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![OpenShift](https://img.shields.io/badge/OpenShift-EE0000?style=for-the-badge&logo=redhatopenshift&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Persistent Volume](https://img.shields.io/badge/Persistent_Storage-4285F4?style=for-the-badge&logo=kubernetes&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

</div>

---

# 📖 Overview

In this lab, you will learn how to deploy and manage **StatefulSets** in Kubernetes/OpenShift.

Unlike Deployments, StatefulSets are designed for applications that require:

- 💾 Persistent Storage
- 🌐 Stable Network Identities
- 🔄 Ordered Deployment & Scaling
- 🗄️ Stateful Data Management

We will deploy a **MySQL StatefulSet** with Persistent Volume Claims (PVCs) and verify data persistence and DNS resolution.

---

# 🎯 Objectives

By the end of this lab, you will be able to:

- ✅ Understand the purpose of StatefulSets
- ✅ Create a StatefulSet YAML manifest
- ✅ Deploy stateful applications
- ✅ Configure Persistent Volume Claims (PVCs)
- ✅ Verify stable Pod identities
- ✅ Test data persistence across Pod recreation
- ✅ Validate StatefulSet networking

---

# 📋 Prerequisites

Before starting this lab, ensure you have:

| Requirement | Description |
|------------|-------------|
| ☸️ Kubernetes Cluster | Minikube, Kind, K3s, EKS, AKS, GKE |
| 🔴 OpenShift Cluster | Optional |
| 🖥️ kubectl / oc CLI | Installed and Configured |
| 💾 StorageClass | Available in Cluster |
| 📘 Kubernetes Basics | Pods, Services, PVCs |
| 📝 YAML Knowledge | Basic Understanding |

---

# 🏗️ StatefulSet Architecture

```text
                        ┌───────────────────┐
                        │ Headless Service  │
                        │      mysql        │
                        └─────────┬─────────┘
                                  │
               ┌──────────────────┼──────────────────┐
               │                                     │
               ▼                                     ▼

      ┌──────────────────┐                ┌──────────────────┐
      │     mysql-0      │                │     mysql-1      │
      │ Stateful Pod     │                │ Stateful Pod     │
      └────────┬─────────┘                └────────┬─────────┘
               │                                   │
               ▼                                   ▼

      ┌──────────────────┐                ┌──────────────────┐
      │ PVC mysql-0      │                │ PVC mysql-1      │
      │ Persistent Data  │                │ Persistent Data  │
      └──────────────────┘                └──────────────────┘
```

---

# 📚 What is a StatefulSet?

A StatefulSet is a Kubernetes workload resource used for managing stateful applications.

### Key Features

✅ Stable Pod Names

✅ Persistent Storage

✅ Ordered Deployment

✅ Ordered Scaling

✅ Stable DNS Entries

### Example Pod Names

```text
mysql-0
mysql-1
mysql-2
```

These names remain consistent even if Pods restart.

---

# 🛠️ Task 1: Write a StatefulSet Manifest

## 🎯 Objective

Create a StatefulSet YAML manifest for a MySQL application with persistent storage.

---

## 📌 Subtask 1.1: Create Manifest File

Create:

```text
mysql-statefulset.yaml
```

Add the following content:

```yaml
apiVersion: apps/v1
kind: StatefulSet

metadata:
  name: mysql

spec:
  serviceName: "mysql"
  replicas: 2

  selector:
    matchLabels:
      app: mysql

  template:
    metadata:
      labels:
        app: mysql

    spec:
      containers:
      - name: mysql
        image: mysql:8.0

        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "password"

        ports:
        - containerPort: 3306
          name: mysql

        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql

  volumeClaimTemplates:
  - metadata:
      name: mysql-persistent-storage

    spec:
      accessModes:
      - ReadWriteOnce

      storageClassName: standard

      resources:
        requests:
          storage: 1Gi
```

---

# 📖 Manifest Explanation

## 🔹 serviceName

Provides stable DNS identity.

```yaml
serviceName: mysql
```

---

## 🔹 replicas

Number of Stateful Pods.

```yaml
replicas: 2
```

---

## 🔹 volumeClaimTemplates

Automatically creates PVCs.

```yaml
volumeClaimTemplates:
```

Each Pod gets its own dedicated storage.

Example:

```text
mysql-persistent-storage-mysql-0
mysql-persistent-storage-mysql-1
```

---

## 🔹 volumeMounts

Mounts storage into container.

```yaml
mountPath: /var/lib/mysql
```

---

## 🔹 MYSQL_ROOT_PASSWORD

Sets MySQL root password.

```yaml
MYSQL_ROOT_PASSWORD=password
```

---

## ✅ Validate Manifest

Run:

```bash
kubectl apply -f mysql-statefulset.yaml --dry-run=client
```

### Expected Output

```text
Manifest validated successfully
```

No validation errors should appear.

---

# 🚀 Task 2: Deploy the Stateful Application

## 🎯 Objective

Deploy StatefulSet and supporting Service.

---

## 📌 Deploy StatefulSet

```bash
kubectl apply -f mysql-statefulset.yaml
```

---

## 📌 Create Headless Service

Create:

```text
mysql-service.yaml
```

Add:

```yaml
apiVersion: v1
kind: Service

metadata:
  name: mysql

spec:
  clusterIP: None

  ports:
  - port: 3306
    name: mysql

  selector:
    app: mysql
```

---

## 📌 Apply Service

```bash
kubectl apply -f mysql-service.yaml
```

---

## 🔍 Verify Resources

```bash
kubectl get statefulset,pods,pvc
```

### Expected Output

```text
NAME                     READY   AGE
statefulset.apps/mysql   2/2     1m

NAME          STATUS    VOLUME
pvc/mysql-0   Bound
pvc/mysql-1   Bound
```

---

# 🌐 Understanding Stable Network Identity

StatefulSets assign permanent DNS names.

### Example Hostnames

```text
mysql-0.mysql
mysql-1.mysql
```

Even if Pods restart:

✅ Names remain unchanged

✅ DNS entries remain unchanged

---

# 🧪 Task 3: Verify Persistent Storage & Network Identity

## 🎯 Objective

Verify:

- Persistent Storage
- Stable Hostnames
- DNS Resolution

---

## 📌 Check Pod Names

```bash
kubectl get pods -l app=mysql -o wide
```

### Expected Output

```text
mysql-0
mysql-1
```

Notice stable naming convention.

---

## 📌 Create Test Database

Connect to mysql-0:

```bash
kubectl exec -it mysql-0 -- \
mysql -uroot -ppassword \
-e "CREATE DATABASE lab_test;"
```

---

## 📌 Delete Pod

```bash
kubectl delete pod mysql-0
```

Kubernetes automatically recreates:

```text
mysql-0
```

with the same identity.

---

## 📌 Verify Persistence

Run:

```bash
kubectl exec -it mysql-0 -- \
mysql -uroot -ppassword \
-e "SHOW DATABASES;"
```

### Expected Output

```text
information_schema
mysql
performance_schema
lab_test
```

✅ Database still exists

✅ Data persisted successfully

---

# 🌐 Verify DNS Resolution

Run:

```bash
kubectl run -it --rm \
--image=busybox:1.28 \
test \
--restart=Never \
-- nslookup mysql
```

### Expected Output

```text
mysql-0.mysql
mysql-1.mysql
```

Stateful DNS records should resolve successfully.

---

# 🏗️ StatefulSet Workflow

```text
Create StatefulSet Manifest
            │
            ▼

Deploy StatefulSet
            │
            ▼

Create Headless Service
            │
            ▼

Provision PVCs
            │
            ▼

Create Stateful Pods
            │
            ▼

Assign Stable Hostnames
            │
            ▼

Persist Application Data
```

---

# 🚨 Troubleshooting Tips

## ⚠️ PVC Stuck in Pending

Check Storage Classes:

```bash
kubectl get storageclass
```

---

### Minikube Users

Enable storage provisioner:

```bash
minikube addons enable storage-provisioner
```

---

## ⚠️ Pods Not Starting

Check Pod Status:

```bash
kubectl get pods
```

Describe Pod:

```bash
kubectl describe pod mysql-0
```

---

## ⚠️ MySQL Errors

Check Logs:

```bash
kubectl logs mysql-0
```

Verify:

```yaml
MYSQL_ROOT_PASSWORD
```

is correctly configured.

---

# 🔍 Useful Commands

## View StatefulSets

```bash
kubectl get statefulsets
```

---

## View Pods

```bash
kubectl get pods
```

---

## View PVCs

```bash
kubectl get pvc
```

---

## View Services

```bash
kubectl get svc
```

---

## View Logs

```bash
kubectl logs mysql-0
```

---

## Describe StatefulSet

```bash
kubectl describe statefulset mysql
```

---

# 🎉 Conclusion

In this lab, you successfully:

✅ Created a StatefulSet manifest

✅ Configured Persistent Volume Claims

✅ Deployed a MySQL Stateful Application

✅ Created a Headless Service

✅ Verified Stable Network Identity

✅ Tested Data Persistence

✅ Validated StatefulSet DNS Resolution

StatefulSets are essential for workloads requiring:

- 💾 Persistent Storage
- 🌐 Stable Hostnames
- 🔄 Ordered Deployment
- 📊 Database Workloads
- 📨 Messaging Systems
- 📁 Distributed Storage

---

# 🧹 Cleanup

Remove all resources:

```bash
kubectl delete -f mysql-statefulset.yaml
kubectl delete -f mysql-service.yaml
```

Verify cleanup:

```bash
kubectl get all
kubectl get pvc
```

---

# 🚀 Next Steps

Explore advanced StatefulSet concepts:

### 🔹 Pod Management Policies

```text
OrderedReady
Parallel
```

### 🔹 Rolling Updates

```yaml
updateStrategy:
  type: RollingUpdate
```

### 🔹 StatefulSet Scaling

```bash
kubectl scale statefulset mysql --replicas=3
```

### 🔹 Volume Expansion

Increase storage capacity dynamically.

### 🔹 Production Database Deployments

- MySQL
- PostgreSQL
- MongoDB
- Cassandra

---

# 📚 Additional Resources

### Kubernetes StatefulSet Documentation

https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/

### Kubernetes Persistent Volumes

https://kubernetes.io/docs/concepts/storage/persistent-volumes/

### OpenShift Stateful Applications

https://docs.openshift.com/

---

<div align="center">

### ☸️ Mastering Kubernetes StatefulSets

**Happy Learning & Happy Kubernetes! 🚀**

</div>
