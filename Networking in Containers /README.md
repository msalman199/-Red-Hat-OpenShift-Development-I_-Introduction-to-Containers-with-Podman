# 🌐 Networking in Containers
## 🎯 Objectives
* **Understand** fundamental container networking structures within rootless Podman environments.
* **Learn** how to map, parse, and inspect complex container network definitions.
* **Practice** launching application tiers utilizing inbound port publishing flags.

---

## 📋 Prerequisites & Setup

### 🔧 Minimum System Requirements
* A Linux-based operating system (**Fedora**, **CentOS**, or **RHEL** are highly recommended).
* Basic structural familiarity executing terminal and shell utility tools.
* **Podman Engine (v4.x+)** installed on the workstation node.
  ```bash
  sudo dnf install podman -y
  ```

---

## 🧪 Lab Execution Steps

### 📌 Task 1: List Podman Networks

#### 🔹 Subtask 1.1: Check Available Networks
Query your system configuration database to view existing network interfaces:
```bash
podman network ls
```
🏁 **Expected Target Output:**
```text
NETWORK ID    NAME        DRIVER
2f259bab93aa  podman      bridge
```
💡 **Key Concept:** By default, Podman sets up a virtual `bridge` configuration layer named `podman` to bridge container communication channels. Additional networks can be appended manually.

#### 🔹 Subtask 1.2: Verify Network Topology Layouts
Inspect the deep-layer configuration schema mapping your primary network stack:
```bash
podman network inspect podman
```
💡 **Key Architectural Concept:** Analyzing this specific configuration block displays active subnet ranges, container gateway IP bindings, and local internal DNS resolvers, which is critical for resolving connectivity faults.

---

### 📌 Task 2: Inspect Network Settings

#### 🔹 Subtask 2.1: Create a Custom Network Layer
Provision a dedicated, logically isolated network infrastructure space to segment your container workloads:
```bash
podman network create lab-network
```

Verify that the network infrastructure subsystem registered your new entry:
```bash
podman network ls | grep lab-network
```

#### 🔹 Subtask 2.2: Inspect Custom Network Metadata
Query the new network interface to retrieve its dynamic properties:
```bash
podman network inspect lab-network
```
🏁 **Expected Outcome:** The console surfaces a detailed JSON block layout outlining the underlying subnet allocations, default gateways, and IPAM operational modes.

🛠️ **Troubleshooting Tip:** If network generation actions return validation errors, inspect your current rootless engine user permissions using `podman info`.

---

### 📌 Task 3: Run Containers with Port Publishing

#### 🔹 Subtask 3.1: Launch a Workload with Port Mapping
Run an isolated background Nginx instance, mapping your local host interface directly onto its target container web engine port:
```bash
podman run -d --name webapp -p 8080:80 docker.io/library/nginx
```
💡 **Structural Parameter Breakout:**
* `-p 8080:80`: Binds ingress traffic hitting host machine port `8080` and redirects it straight to internal container port `80`.
* `-d`: Detaches the container process execution line, keeping it running quietly in the background.

#### 🔹 Subtask 3.2: Verify Port Accessibility
Execute a network connectivity test to check your exposed endpoint:
```bash
curl http://localhost:8080
```
🏁 **Expected Output:** Returns the raw HTML source of the default Nginx index welcome landing page.

🛠️ **Troubleshooting Tip:** If connection requests hang or timeout, run `podman ps` to check that the container process is healthy, and confirm your local OS firewalls allow traffic through that port (`sudo firewall-cmd --list-ports`).

#### 🔹 Subtask 3.3: Attach Container to the Custom Network Space
To implement secure infrastructure partitioning, swap the web container over into your isolated lab network layer:
```bash
podman stop webapp
podman run -d --name webapp -p 8080:80 --network lab-network docker.io/library/nginx
```

Confirm that the application runtime has been successfully joined to the correct isolated segment:
```bash
podman inspect webapp | grep -A 5 "Networks"
```

---

## 🎉 Conclusion & Key Takeaways
In this lab scenario deployment, you successfully achieved the following:
* **Explored** Podman's engine components across default networks and custom overlays.
* **Inspected** inner-layer topological JSON structural blocks for network fault isolation.
* **Published** application container delivery target listener ports out onto public host sockets.

### 🔑 Critical Takeaways
* **Default Bridge Routing:** Podman assigns workloads to a default bridge configuration unless explicit environment arguments instruct it otherwise.
* **Ingress Mapping:** The `-p` port translation parameters are completely vital to make hidden application resources accessible to outer clients.
* **Network Isolation:** Provisioning custom virtual network switches lets you create secure, air-gapped zones for sensitive app components.

---

## 🚀 Next Steps
* Experiment orchestrating multi-tier applications by utilizing the `podman network connect` utility command to bridge communication pipelines across distinct running container containers.

⚠️ ***Note:** All configuration sequences outlined above are validated for **Podman v4.x+** deployment specifications. For legacy engine variations, consult the official [Podman Documentation](https://docs.podman.io).*
