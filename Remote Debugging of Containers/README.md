# 🔍 Remote Debugging of Containers 
## 🎯 Objectives
* **Set up** remote debugging hook links for running containers without rebuilding your final production image layers.
* **Expose** language-specific runtime debug engine listener sockets safely across isolated network barriers.
* **Mount** working local application source paths directly into container execution targets to enable hot-reloading.
* **Connect** an interactive IDE debugger framework (VS Code) to capture active container processes and parse breakpoints.

---

## 📋 Prerequisites & Setup

### 🔧 Minimum System Requirements
* Basic functional command-line terminal and shell environment operations knowledge.
* **Podman** or **Docker** installed (Podman preferred for Red Hat OpenShift alignment paths).
* **VS Code** desktop code editor installed with the native **"Dev Containers"** (formerly Remote - Containers) extension active.
* A target sample application script stack (e.g., a basic Node.js project or Python Flask file) packaged for debugging.

---

## 🧪 Lab Execution Steps

### 📌 Task 1: Expose Debug Ports

To allow external editors to pause and step through application threads, you must open specific debugging channel routes through the container runtime firewall layer.

#### 🔹 Subtask 1.1: Identify Debug Port Targets for Your Specific Language stack
Different compilation architectures and execution engines run custom diagnostic network sockets:
* 🟢 **Node.js:** Port `9229` *(invoked using the specialized engine flag `--inspect`)*
* 🟡 **Python:** Port `5678` *(commonly utilized by the `debugpy` tool framework)*
* 🔵 **Java:** Port `5005` *(native socket assigned to the Java Debug Wire Protocol - JDWP)*

#### 🔹 Subtask 1.2: Launch Your App Container with Open Debug Target Paths
Spin up a test instance passing the explicit external management listener port values down to your engine routing stack:
```bash
podman run -d -p 3000:3000 -p 9229:9229 --name debug-container my-node-app
```
💡 **Structural Parameter Breakout:**
* `-p 9229:9229`: Captures internal application debugging communications and routes them directly out to your local host adapter block.
* Replace `my-node-app` with your actual local code base template compilation tag name.

🏁 **Expected Outcome:** The target test container launches cleanly in the background with both its consumer traffic web port (`3000`) and inspector sockets active.

🛠️ **Troubleshooting Tip:** If your console errors out stating that a port allocation is already blocked or bound, change your host intercept variable values mapping onward (e.g., utilize `-p 9230:9229`).

---

### 📌 Task 2: Mount Source Code for Live Debugging

#### 🔹 Subtask 2.1: Spin Up the Workload Container with Volume Overlays
Inject a live host sync mount path using volume arguments to instantly mirror your workspace updates without triggering a manual rebuild loop:
```bash
podman run -d -p 3000:3000 -p 9229:9229 -v \$(pwd):/app --name debug-container my-node-app
```
💡 **Key Concept:** The `$(pwd):/app` parameter runtime configuration creates a transparent bridge between your host computer's active directory (`print working directory`) and the isolated target path folder `/app` within the running container.

#### 🔹 Subtask 2.2: Verify Mounted Files Inside the Sandbox Context
Validate file alignment using runtime exec file listings inside the running container workspace:
```bash
podman exec -it debug-container ls /app
```
🏁 **Expected Outcome:** The screen lists your exact local development files, verifying that the file system bridge is up and working.

🛠️ **Troubleshooting Tip:** If directory list queries return blank strings or path formatting issues, double-check that your absolute workspace file paths are correct.

---

### 📌 Task 3: Connect IDE Debugger to Container

#### 🔹 Subtask 3.1: Install VS Code Remote Debugging Tools
1. Access the **Extensions Panel** within your VS Code interface (`Ctrl+Shift+X`).
2. Search for and install the official **Dev Containers** toolset extension package.
3. For target Node.js microservice configurations, ensure the native **Debugger for Node.js** integration layer is enabled.

#### 🔹 Subtask 3.2: Attach Debugger Engine Hooks to the Running Workspace
1. Launch VS Code over your project repository folder and navigate to the **Run and Debug** left-side panel view layer (`Ctrl+Shift+D`).
2. Select **"Create a launch.json file"** link text and pick **Node.js** as your environment format.
3. Replace the auto-generated configuration file block structure with the following target mapping array:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "attach",
      "name": "Attach to Container",
      "address": "localhost",
      "port": 9229,
      "localRoot": "\${workspaceFolder}",
      "remoteRoot": "/app"
    }
  ]
}
```

4. Trigger active connection capturing by hitting the green **Play icon button** (or pressing your `F5` keyboard short key).

🏁 **Expected Outcome:** VS Code links up directly with the target listening container process. Setting a breakpoint on a code row turns solid red and actively pauses runtime executions when paths trigger.

🛠️ **Troubleshooting Tip:** If attachment validation timeouts block your interface connection, check your firewall statuses and verify using `podman ps` that your target test container hasn't stopped.

---

## 🎉 Conclusion & Key Takeaways
In this lab scenario deployment, you successfully achieved the following:
* **Exposed** secure backplane debugger network sockets from within runtime workloads.
* **Mapped** hot-swappable project storage directory volumes straight into isolated image namespaces.
* **Integrated** local IDE engineering workflows (VS Code) with un-compiled remote runtime containers.

### 🔑 Critical Takeaways
* This agile approach prevents you from constantly rebuilding images during development loops, speeding up troubleshooting cycles.
* Ensuring mapping alignments across `localRoot` and `remoteRoot` paths allows your local editor to trace line breaks and values correctly.

---

## 🚀 Next Steps
* Replicate this advanced inspection technique with other language backends like **Python (via debugpy)** or **Java (via JDWP frameworks)**.
* Explore how cloud developers translate these local connections into enterprise cluster scopes like **Red Hat OpenShift Port Forwarding** commands.
