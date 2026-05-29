# 🔍 Troubleshooting Containers
## 🎯 Objectives
* **Diagnose** abrupt or hidden container runtime failures using structured log streaming and metadata inspect arrays.
* **View** and isolate container event stdout/stderr channels using time-based and line-count filters.
* **Inspect** deep-layer container process states, resource allocations, and network configurations.
* **Execute** an interactive diagnostic terminal trace session inside running isolated environments.

---

## 📋 Prerequisites & Setup

### 🔧 Minimum System Requirements
* Basic baseline understanding of command-line interface execution routines.
* **Podman** or **Docker** installed (Podman recommended for Red Hat Enterprise Linux and OpenShift compatibility workflows).

### 🛠️ Verification & Initial Lab Initialization

1. **Install Podman (if missing from your current control station node):**
   * *For RHEL / CentOS / Fedora systems:*
     ```bash
     sudo dnf install -y podman
     ```
   * *For Debian / Ubuntu systems:*
     ```bash
     sudo apt-get install -y podman
     ```

2. **Pull the target web server testing image layer:**
   ```bash
   podman pull docker.io/library/nginx:alpine
   ```

3. **Launch the baseline testing workload thread in background detached mode:**
   ```bash
   podman run -d --name nginx-test -p 8080:80 nginx:alpine
   ```

---

## 🧪 Lab Execution Steps

### 📌 Task 1: View Container Logs with Filters

#### 🔹 Subtask 1.1: View Basic Log Streams
Fetch the standard diagnostic text outputs from your container:
```bash
podman logs nginx-test
```
🏁 **Expected Output:** Displays historical Nginx initialization entries along with traffic access records.

Stream runtime output traces continuously to monitor active connections:
```bash
podman logs -f nginx-test
```
🏁 **Expected Outcome:** Terminal lines hold open and update live. Press `Ctrl + C` on your keyboard to drop out of the stream.

#### 🔹 Subtask 1.2: Filter Logs by Specific Time Horizons
Query output streams relative to an explicit time window to narrow down recent errors:
```bash
podman logs --since 5m nginx-test
```
💡 **Key Concept:** This command ignores ancient history and narrows your troubleshooting window down strictly to the last 5 minutes of operations.

Isolate and fetch a quick snapshot of the absolute latest log lines:
```bash
podman logs --tail 10 nginx-test
```
🏁 **Expected Outcome:** The screen clips all old metrics, outputting exactly the last 10 entries registered by the application.

🛠️ **Troubleshooting Tip:** If logs return empty arrays, execute `podman ps` to confirm the application didn't crash or stop abruptly due to configuration errors.

---

### 📌 Task 2: Inspect Container State

#### 🔹 Subtask 2.1: Check Container Operational Status
List active execution states to check if your container is running or crashed:
```bash
podman ps
```
🏁 **Expected Output:** Shows the `nginx-test` workload entry registered inside the active running table list.

Extract the absolute underlying metadata config block engine records:
```bash
podman inspect nginx-test
```
🔍 **Critical Fields to Evaluate for Fault Resolution:**
* `State`: Displays status tracking strings (e.g., `Running`), the exit code (`ExitCode`), and failure errors.
* `Config`: Isolates applied environment metrics, storage boundaries, and entrypoint executables.
* `NetworkSettings`: Evaluates dynamically mapped IP allocations, internal subnets, and active exposed ports.

#### 🔹 Subtask 2.2: Check Runtime Resource Footprints
Monitor infrastructure processing performance metrics in real time:
```bash
podman stats nginx-test
```
🏁 **Expected Outcome:** Provides a live console display showing host CPU utility percentage, real-time memory caps, and cumulative network IO blocks.

🛠️ **Troubleshooting Tip:** If a container stops responding or is stuck in an infinite processing loop, execute a hard stop sequence and re-initialize it: `podman stop nginx-test && podman start nginx-test`.

---

### 📌 Task 3: Use exec to Debug Inside Container

#### 🔹 Subtask 3.1: Launch an Interactive Internal Shell Session
Bypass external barriers to connect into the application namespace:
```bash
podman exec -it nginx-test /bin/sh
```
💡 **Key Parameter Concept:** The `-it` interactive tty flag switches your workstation shell straight into the active filesystem environment of the isolated container.

Investigate active processes executing within the isolated container sandbox:
```sh
ps aux
```
🏁 **Expected Output:** Displays active processing records proving the Nginx master engine and worker threads are running.

#### 🔹 Subtask 3.2: Debug Server Service Configurations
Read active layout structures to check for server config format alignment errors:
```sh
cat /etc/nginx/nginx.conf
```

Verify internal processing engine responsiveness using curl loops:
```sh
curl localhost
```
🏁 **Expected Output:** Returns raw HTML text block layouts tracking the standard default Nginx greeting screen page.

To return back out to your main host system shell, type:
```sh
exit
```

🛠️ **Troubleshooting Tip:** If the standard `/bin/sh` call parameter exits instantly with execution failures, check if alternate shell interpreters exist inside your image (e.g., `podman exec -it nginx-test /bin/bash`).

---

## 🎉 Conclusion & Review
In this lab scenario, you successfully implemented the following diagnostic processes:
* **Filtered** diagnostic output log channels using specific time limits and targeted line boundaries.
* **Parsed** the internal metadata arrays of container structures via detailed inspection actions.
* **Monitored** system metrics across processing nodes, tracking memory boundaries and CPU metrics.
* **Interacted** directly with internal configurations using active exec terminal hooks to run live connectivity tests.

---

## 🚀 Next Steps & Advanced Scenarios
* Practice debugging a container configuration that intentionally crashes on startup (e.g., passing invalid flags) by analyzing the exit codes.
* Run `podman system df` to check your host infrastructure storage consumption metrics across container volumes and caching blocks.

---

## 🧹 Workspace Cleanup
Run the cleanup sequence below to clear your laboratory infrastructure trace metrics and liberate local ports:

```bash
podman stop nginx-test
podman rm nginx-test
```

---

### 📝 Lab Completion Checklist
- [ ] Viewed container logs using targeted filtering criteria.
- [ ] Inspected detailed container runtime states and system resources.
- [ ] Executed an internal diagnostic debug session using interactive commands.

💬 **Feedback Assessment:** How would you score your experience running this laboratory scenario? (**Easy** / **Medium** / **Hard**). What specific troubleshooting challenges did you encounter?
