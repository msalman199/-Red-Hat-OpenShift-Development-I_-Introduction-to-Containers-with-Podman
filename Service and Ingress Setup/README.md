# 🌐 Kubernetes Service and Ingress Setup 

<div align="center">

# ☸️ Service and Ingress Setup

### Expose Applications Internally and Externally in Kubernetes & OpenShift

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![OpenShift](https://img.shields.io/badge/OpenShift-EE0000?style=for-the-badge&logo=redhatopenshift&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![Ingress](https://img.shields.io/badge/Ingress-4285F4?style=for-the-badge&logo=kubernetes&logoColor=white)
![Services](https://img.shields.io/badge/Kubernetes_Service-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)

</div>

---

# 📖 Overview

In Kubernetes and OpenShift, applications are typically deployed as Pods and Deployments. To make these applications accessible, Kubernetes provides several networking resources:

- 🔹 ClusterIP Services
- 🔹 NodePort Services
- 🔹 Ingress Resources
- 🔹 OpenShift Routes

This lab demonstrates how to expose applications both internally and externally.

---

# 🎯 Objectives

By the end of this lab, you will be able to:

- ✅ Expose applications using ClusterIP Services
- ✅ Expose applications using NodePort Services
- ✅ Configure OpenShift Routes
- ✅ Configure Kubernetes Ingress Resources
- ✅ Validate application accessibility
- ✅ Troubleshoot Service and Ingress issues

---

# 📋 Prerequisites

Before starting this lab, ensure you have:

| Requirement | Description |
|------------|------------|
| ☸️ Kubernetes Cluster | Minikube, Kind, EKS, AKS, GKE, etc. |
| 🔴 OpenShift Cluster | Optional |
| 🖥️ kubectl / oc CLI | Installed and configured |
| 📘 Kubernetes Basics | Pods, Deployments, Services |
| 🌐 Sample Application | Nginx or Web Application |

---

# 🏗️ Service Architecture

```text
                     External User
                            │
                            ▼

                 ┌────────────────────┐
                 │ Route / Ingress    │
                 └──────────┬─────────┘
                            │
                            ▼

                 ┌────────────────────┐
                 │  NodePort Service  │
                 └──────────┬─────────┘
                            │
                            ▼

                 ┌────────────────────┐
                 │ ClusterIP Service  │
                 └──────────┬─────────┘
                            │
                            ▼

                 ┌────────────────────┐
                 │    Nginx Pod       │
                 └────────────────────┘
```

---

# 🚀 Task 1: Create ClusterIP and NodePort Services

## 🎯 Objective

Deploy a sample application and expose it using different Kubernetes Service types.

---

# 📌 Subtask 1.1: Deploy a Sample Application

Create an Nginx deployment:

```bash
kubectl create deployment nginx --image=nginx:latest
```

### Expected Output

```bash
deployment.apps/nginx created
```

---

## 🔍 Verify Deployment

Check deployments:

```bash
kubectl get deployments
```

---

Check Pods:

```bash
kubectl get pods
```

### Expected Output

```text
NAME                     READY   STATUS
nginx-xxxxxxxxxx-xxxxx   1/1     Running
```

---

# 📌 Subtask 1.2: Create a ClusterIP Service

A ClusterIP Service exposes an application internally within the cluster.

---

## ▶️ Create ClusterIP Service

```bash
kubectl expose deployment nginx \
--port=80 \
--type=ClusterIP \
--name=nginx-clusterip
```

### Expected Output

```bash
service/nginx-clusterip exposed
```

---

## 🔍 Verify Service

```bash
kubectl get svc nginx-clusterip
```

### Expected Output

```text
NAME              TYPE        CLUSTER-IP      PORT(S)
nginx-clusterip   ClusterIP   10.96.123.45    80/TCP
```

---

# 📚 Understanding ClusterIP

### Features

✅ Internal Cluster Access

✅ Default Service Type

✅ Not Accessible Outside Cluster

---

### Traffic Flow

```text
Pod → ClusterIP Service → Pod
```

---

# 📌 Subtask 1.3: Create a NodePort Service

A NodePort Service exposes an application through a port on every worker node.

---

## ▶️ Create NodePort Service

```bash
kubectl expose deployment nginx \
--port=80 \
--type=NodePort \
--name=nginx-nodeport
```

### Expected Output

```bash
service/nginx-nodeport exposed
```

---

## 🔍 Verify Service

```bash
kubectl get svc nginx-nodeport
```

### Expected Output

```text
NAME             TYPE       CLUSTER-IP      PORT(S)
nginx-nodeport   NodePort   10.96.234.56    80:30007/TCP
```

---

## 🌐 Access Application

Find Node IP:

```bash
kubectl get nodes -o wide
```

Access:

```bash
curl http://<NODE_IP>:30007
```

Replace:

```text
<NODE_IP>
```

with your worker node IP.

---

### Expected Result

🎉 Nginx Welcome Page

---

# 📚 Understanding NodePort

### Features

✅ External Access

✅ Static Port

✅ Accessible on Every Node

---

### Default Port Range

```text
30000 - 32767
```

---

### Traffic Flow

```text
Internet
   │
   ▼

NodeIP:30007
   │
   ▼

NodePort Service
   │
   ▼

Nginx Pod
```

---

# 🚀 Task 2: Configure OpenShift Route or Kubernetes Ingress

## 🎯 Objective

Expose applications externally using Routes or Ingress resources.

---

# 🔴 Option A: OpenShift Route

(OpenShift Users)

---

## 📌 Subtask 2.1: Create Route

```bash
oc expose svc nginx-nodeport --name=nginx-route
```

### Expected Output

```bash
route.route.openshift.io/nginx-route exposed
```

---

## 🔍 Verify Route

```bash
oc get route nginx-route
```

### Expected Output

```text
NAME          HOST/PORT
nginx-route   nginx-route-default.example.com
```

---

## 🌐 Access Route

```bash
curl http://nginx-route-default.example.com
```

### Expected Result

🎉 Nginx Welcome Page

---

# 📚 Understanding Routes

### Features

✅ OpenShift Native

✅ DNS-Based Access

✅ SSL/TLS Support

✅ Simplified External Exposure

---

# ☸️ Option B: Kubernetes Ingress

(Kubernetes Users)

---

## 📌 Subtask 2.2: Install Ingress Controller

If not already installed:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

---

## 🔍 Verify Controller

```bash
kubectl get pods -n ingress-nginx
```

Expected:

```text
Running
```

---

## 📌 Create Ingress Resource

Create:

```text
nginx-ingress.yaml
```

Add:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: nginx-ingress

spec:
  rules:
  - host: nginx.example.com

    http:
      paths:
      - path: /

        pathType: Prefix

        backend:
          service:
            name: nginx-nodeport
            port:
              number: 80
```

---

## ▶️ Deploy Ingress

```bash
kubectl apply -f nginx-ingress.yaml
```

### Expected Output

```bash
ingress.networking.k8s.io/nginx-ingress created
```

---

## 🔍 Verify Ingress

```bash
kubectl get ingress nginx-ingress
```

### Expected Output

```text
NAME            HOSTS
nginx-ingress   nginx.example.com
```

---

## 🌐 Configure Local DNS

Get Ingress IP:

```bash
kubectl get ingress
```

Update hosts file:

```bash
echo "<INGRESS_IP> nginx.example.com" | sudo tee -a /etc/hosts
```

---

## 🌍 Test Application

```bash
curl http://nginx.example.com
```

### Expected Result

🎉 Nginx Welcome Page

---

# 📚 Understanding Ingress

### Features

✅ HTTP Routing

✅ HTTPS Support

✅ Host-Based Routing

✅ Path-Based Routing

✅ Load Balancing

---

### Traffic Flow

```text
Internet
    │
    ▼

nginx.example.com
    │
    ▼

Ingress Controller
    │
    ▼

Service
    │
    ▼

Nginx Pod
```

---

# 🔍 Useful Commands

## List Services

```bash
kubectl get svc
```

---

## Describe Service

```bash
kubectl describe svc nginx-nodeport
```

---

## List Ingress Resources

```bash
kubectl get ingress
```

---

## Describe Ingress

```bash
kubectl describe ingress nginx-ingress
```

---

## View Routes (OpenShift)

```bash
oc get routes
```

---

# 🚨 Troubleshooting Tips

## ⚠️ NodePort Not Accessible

Verify Service:

```bash
kubectl describe svc nginx-nodeport
```

Check:

- Firewall Rules
- Security Groups
- Worker Node Reachability

---

## ⚠️ Ingress Not Working

Verify Controller:

```bash
kubectl get pods -n ingress-nginx
```

---

Check Logs:

```bash
kubectl logs -n ingress-nginx <ingress-controller-pod>
```

---

Verify Resource:

```bash
kubectl describe ingress nginx-ingress
```

---

## ⚠️ Route Not Accessible

Verify Route:

```bash
oc get route
```

Check Router Pods:

```bash
oc get pods -n openshift-ingress
```

---

# 🔥 Service Types Comparison

| Feature | ClusterIP | NodePort | Ingress | Route |
|----------|-----------|-----------|----------|---------|
| Internal Access | ✅ | ✅ | ✅ | ✅ |
| External Access | ❌ | ✅ | ✅ | ✅ |
| DNS Support | ❌ | ❌ | ✅ | ✅ |
| HTTPS Support | ❌ | ❌ | ✅ | ✅ |
| Load Balancing | ✅ | ✅ | ✅ | ✅ |

---

# 🎉 Conclusion

In this lab, you successfully:

✅ Created a Deployment

✅ Exposed applications using ClusterIP

✅ Exposed applications using NodePort

✅ Configured OpenShift Routes

✅ Configured Kubernetes Ingress

✅ Tested internal and external application access

These networking concepts are essential for exposing applications in production Kubernetes environments.

---

# 🚀 Next Steps

Continue learning:

- 🔹 Kubernetes LoadBalancer Services
- 🔹 TLS Certificates
- 🔹 Ingress Annotations
- 🔹 NGINX Ingress Controller
- 🔹 OpenShift Router Configuration
- 🔹 Service Mesh (Istio)
- 🔹 Advanced Traffic Routing

---

# 📚 Additional Resources

### Kubernetes Services Documentation

https://kubernetes.io/docs/concepts/services-networking/service/

### Kubernetes Ingress Documentation

https://kubernetes.io/docs/concepts/services-networking/ingress/

### OpenShift Routes Documentation

https://docs.openshift.com/container-platform/latest/networking/routes/
### ☸️ Mastering Kubernetes Networking!

**Happy Learning & Happy Deploying! 🚀**

</div>
