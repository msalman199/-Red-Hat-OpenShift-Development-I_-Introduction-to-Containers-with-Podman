# 🌐 Environment Variables in Image

## 🎯 Objectives
* **Use** `ENV` and `ARG` instructions in Containerfiles to manage external configuration.
* **Define** persistent environment variables directly inside container images.
* **Override** preset environment configurations dynamically at container runtime.
* **Inspect** environment variable assignments inside live, running containers.

---

## 📋 Prerequisites & Setup

### 🔧 Minimum System Requirements
* Basic Linux command-line knowledge.
* Text editor access (`Vim`, `Nano`, or `VS Code`).
* Internet access to pull official Red Hat Universal Base Images (`UBI`).
* **Podman** or **Docker** installed (Podman recommended for Red Hat/OpenShift alignment).

### 🛠️ Verification & Initial Setup

1. **Install Podman (on RHEL/Fedora/CentOS Stream):**
   ```bash
   sudo dnf install podman -y
   ```

2. **Verify your installation:**
   ```bash
   podman --version
   ```

3. **Create and enter your dedicated work directory:**
   ```bash
   mkdir env-lab && cd env-lab
   ```

---

## 🧪 Lab Execution Steps

### 📌 Task 1: Define Environment Variables in Containerfile

#### 🔹 Subtask 1.1: Create a Basic Containerfile
Generate a new build manifest file named `Containerfile` containing baseline application configurations:

```bash
cat <<EOF > Containerfile
FROM registry.access.redhat.com/ubi8/ubi-minimal
ENV APP_NAME="MyApp" \\
    APP_VERSION="1.0" \\
    APP_ENV="development"
CMD echo "Running \$APP_NAME v\$APP_VERSION in \$APP_ENV mode"
EOF
```

#### 🔹 Subtask 1.2: Build and Run the Image
Compile your container configuration:
```bash
podman build -t env-demo .
```

Execute a new container instance from the compiled image layer:
```bash
podman run env-demo
```
🏁 **Expected Output:**
```text
Running MyApp v1.0 in development mode
```

---

### 📌 Task 2: Override Environment Variables at Runtime

#### 🔹 Subtask 2.1: Override Using Command Line Flags
Inject new variables directly into the application instance using the inline runtime execution flag `-e`:

```bash
podman run -e APP_ENV="production" -e APP_VERSION="2.0" env-demo
```
🏁 **Expected Output:**
```text
Running MyApp v2.0 in production mode
```

#### 🔹 Subtask 2.2: Use Environment Configuration Files
Create a standalone configuration context key-value configuration file named `app.env`:

```bash
cat <<EOF > app.env
APP_NAME=ProductionApp
APP_VERSION=3.0
APP_ENV=staging
EOF
```

Launch the runtime configuration container pointing directly to your bulk environment configuration text file:
```bash
podman run --env-file=app.env env-demo
```
🏁 **Expected Outcome:**
```text
Running ProductionApp v3.0 in staging mode
```

---

### 📌 Task 3: Inspect Variables in Running Containers

#### 🔹 Subtask 3.1: View Live Environment Variables
Launch the workload in background detached mode (`-d`) to explore active settings:

```bash
podman run -d --name env-container env-demo
```

Query the active memory configuration block inside the isolated context using `exec`:
```bash
podman exec env-container env
```
🏁 **Expected Output (Partial Match):**
```text
APP_NAME=MyApp
APP_VERSION=1.0
APP_ENV=development
```

#### 🔹 Subtask 3.2: Use ARG for Build-Time Variables
Overwrite your `Containerfile` to test transient build execution variables using the `ARG` parameter instruction:

```bash
cat <<EOF > Containerfile
FROM registry.access.redhat.com/ubi8/ubi-minimal
ARG APP_BUILD_NUMBER
ENV APP_BUILD=\$APP_BUILD_NUMBER
CMD echo "Build number: \$APP_BUILD"
EOF
```

Compile your image layout while dynamically piping structural values via the `--build-arg` command switch:
```bash
podman build --build-arg APP_BUILD_NUMBER=42 -t arg-demo .
```

Verify your artifact configuration values:
```bash
podman run arg-demo
```
🏁 **Expected Output:**
```text
Build number: 42
```

---

## 🛠️ Troubleshooting Tips
* ❌ **Variables missing or parsing poorly?** Verify your formatting syntax layout. Avoid placing arbitrary execution spaces around `=` identifiers when using `ENV`.
* ❌ **ARG changes not updating layers?** Ensure arguments are passed during creation utilizing the explicit `--build-arg` identifier flag.
* ❌ **Permission failures?** Run Podman operations using `sudo` flags if local access controls require it. *(Note: This rootful approach is not standard pattern for enterprise OpenShift production environments)*

---

## 🎉 Conclusion
In this lab scenario deployment, you successfully achieved the following:
* **Defined** long-term application parameters with `ENV` flags in your Containerfile configurations.
* **Overrode** predefined variables globally using targeted runtime inline commands `-e` and `--env-file`.
* **Inspected** variable mapping details of active background container instances.
* **Integrated** compilation-phase parameter handling variables with `ARG` definitions.

---

## 🚀 Next Steps
* Experiment creating advanced multi-stage build workflows with dynamically evaluated `ARG` logic components.
* Explore structural integrations where OpenShift platforms reference environments automatically inside production Deployment manifests.
* Practice mapping variables via platform configuration storage arrays like Kubernetes **ConfigMaps** and **Secrets**.

---

## 🧹 Workspace Cleanup
Run the following cleanup sequence to purge all container records and release memory limits:

```bash
podman rm -f \$(podman ps -aq)
podman rmi env-demo arg-demo
```
