# 🏁 Using ENTRYPOINT and CMD in Containerfiles
## 🎯 Objectives
* **Understand** the structural difference between `ENTRYPOINT` and `CMD` instructions.
* **Configure** container startup processes using standard execution structures.
* **Test** runtime container behavior under varying flag combinations.
* **Override** default container commands seamlessly during runtime invocation.

---

## 📋 Prerequisites & Setup

### 🔧 Minimum System Requirements
* Terminal or command-line execution environment access.
* Basic baseline understanding of Containerfile / Dockerfile keywords.
* **Podman** or **Docker** installed (Podman recommended for Red Hat/OpenShift ecosystems).

### 🛠️ Workspace Preparation
Create and switch into your clean working directory infrastructure:
```bash
mkdir entrypoint-cmd-lab && cd entrypoint-cmd-lab
```

---

## 🧪 Lab Execution Steps

### 📌 Task 1: Writing a Containerfile with ENTRYPOINT and CMD

#### 🔹 Subtask 1.1: Create a Basic Containerfile
Generate a text file named `Containerfile` with the following baseline execution block:

```dockerfile
# Base image
FROM registry.access.redhat.com/ubi9/ubi-minimal

# Set ENTRYPOINT to a shell script
ENTRYPOINT ["echo", "Entrypoint says:"]

# Set default CMD arguments
CMD ["Default CMD message"]
```

💡 **Architectural Concept Breakout:**
* `ENTRYPOINT`: Declares the absolute main immutable executable framework binary running at start.
* `CMD`: Appends default string arguments passed down into your entrypoint block.
* *Combined Translation:* `echo "Entrypoint says:" "Default CMD message"`

#### 🔹 Subtask 1.2: Build the Container Image
Compile your container configurations utilizing the Podman engine context:
```bash
podman build -t entrypoint-demo .
```
🏁 **Expected Target Compilation Output:**
```text
STEP 1/3: FROM registry.access.redhat.com/ubi9/ubi-minimal
STEP 2/3: ENTRYPOINT ["echo", "Entrypoint says:"]
STEP 3/3: CMD ["Default CMD message"]
COMMIT entrypoint-demo
```

---

### 📌 Task 2: Testing Container Behavior

#### 🔹 Subtask 2.1: Run with Default Runtime Arguments
Execute the container instance with automatic short-lived cleanups (`--rm`):
```bash
podman run --rm entrypoint-demo
```
🏁 **Expected Output:**
```text
Entrypoint says: Default CMD message
```

#### 🔹 Subtask 2.2: Override CMD at Runtime
Pass an arbitrary string parameter directly immediately following your tracking image tag name:
```bash
podman run --rm entrypoint-demo "Custom message"
```
🏁 **Expected Output:**
```text
Entrypoint says: Custom message
```
💡 **Key Architectural Concept:** Any extra trailing arguments declared at the CLI replace your hardcoded image `CMD` array block wholesale.

---

### 📌 Task 3: Advanced ENTRYPOINT/CMD Combinations

#### 🔹 Subtask 3.1: Create a Script-Based ENTRYPOINT
Write an external target application launcher wrapper script file named `greet.sh`:
```bash
cat <<'EOF' > greet.sh
#!/bin/sh
echo "Welcome to $1 from $2"
EOF
```

Grant executable filesystem workspace file rights:
```bash
chmod +x greet.sh
```

Update your core `Containerfile` manifest layer targeting your system execution script:
```dockerfile
FROM registry.access.redhat.com/ubi9/ubi-minimal

COPY greet.sh /usr/local/bin/
ENTRYPOINT ["/usr/local/bin/greet.sh"]
CMD ["OpenShift Lab", "Red Hat"]
```

Compile the updated runtime demo layers:
```bash
podman build -t greet-demo .
```

#### 🔹 Subtask 3.2: Test the Script-Based Container
Run the script-driven container tracking defaults:
```bash
podman run --rm greet-demo
```
🏁 **Expected Output:**
```text
Welcome to OpenShift Lab from Red Hat
```

Inject explicit secondary values to verify parameter array shifting:
```bash
podman run --rm greet-demo "Container Workshop" "Instructor"
```
🏁 **Expected Output:**
```text
Welcome to Container Workshop from Instructor
```

---

### 📌 Task 4: Overriding ENTRYPOINT

#### 🔹 Subtask 4.1: Use the --entrypoint Flag
Bypass locked entrypoint frameworks completely using the explicit CLI global `--entrypoint` switch:
```bash
podman run --rm --entrypoint echo greet-demo "This completely replaces the ENTRYPOINT"
```
🏁 **Expected Output:**
```text
This completely replaces the ENTRYPOINT
```
🛠️ **Troubleshooting Tip:** If scripts throw permission execution errors (`Permission Denied`), confirm executable rights are explicitly set on the host file (`chmod +x`) *prior* to processing container image layer context packaging actions.

---

### 📌 Task 5: Understanding Shell vs Exec Form

#### 🔹 Subtask 5.1: Create a Shell-Form Example
Modify your config metadata structure to observe text-parsing modifications:
```dockerfile
FROM registry.access.redhat.com/ubi9/ubi-minimal

# Shell form (note: not recommended for most production tracking cases)
ENTRYPOINT echo "Shell form ENTRYPOINT:"
CMD echo "Shell form CMD"
```

🔍 **Key Structural Differences:**
* **Exec Form** (`["json", "array"]`): Standard target system calls execute directly without system environment translations. Recommended for passing signals (`SIGTERM`) straight into application runtimes.
* **Shell Form** (`string entry`): Invisibly wraps applications beneath a local layer instance process path via `/bin/sh -c`, preventing proper OS signal parsing and child process scaling behaviors.

---

## 🎉 Conclusion & Best Practices
In this lab scenario, you mastered the following:
* **Identified** the operational boundary definitions tracking `ENTRYPOINT` and `CMD` components.
* **Merged** entrypoints with scripts for clean environment pre-flight checking procedures.
* **Overrode** application configurations easily using runtime arguments and target override switches.

### 🔑 Recommended Best Practices
* Always utilize **Exec Form (JSON Array layout)** formatting styles for standard directives.
* Constrain `CMD` strictly to basic fallback variable inputs that are easy to overwrite.
* Leverage wrapper setup scripts with your entrypoint tracking to handle sophisticated runtime calculations cleanly.

---

## 🚀 Additional Exercises & Exploration
* Try building a custom target tracking instance executing a micro **Python script** triggered by an entrypoint wrapper passing options with `CMD`.
* Integrate global system definitions by merging combinations across `ENTRYPOINT`, `CMD`, and `ENV` states.
* Explore how enterprise OpenShift container systems map tracking overrides on workloads using manifest deployments blocks.

---

## 🧹 Workspace Cleanup
Purge active build tracking file items from local storage pools to free resources:
```bash
podman rmi entrypoint-demo greet-demo
```
