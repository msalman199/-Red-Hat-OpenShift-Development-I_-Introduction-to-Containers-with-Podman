# 🗄️ Running Stateful Containers 
## 🎯 Objectives
* **Understand** and implement persistent storage structures for stateful workloads.
* **Deploy** enterprise MySQL and PostgreSQL containers with explicit volume mount layers.
* **Verify** application state and table record survival across complete container lifecycles.

---

## 📋 Prerequisites & Setup

### 🔧 Minimum System Requirements
* Basic baseline proficiency running terminal commands.
* Minimum **2GB+ of free workspace disk storage space**.
* Active Internet access to pull database container image layers.
* **Podman** or **Docker** runtime engine engine installed (Podman recommended for Red Hat/OpenShift alignment).

### 🛠️ Verification & Environment Preparation

1. **Verify your container engine installation:**
   ```bash
   podman --version
   ```
   🏁 **Expected Output:** Reports system engine version metrics (e.g., `4.0.0` or higher).

2. **Establish your isolated laboratory working directory paths:**
   ```bash
   mkdir -p ~/stateful-lab && cd ~/stateful-lab
   ```

---

## 🧪 Lab Execution Steps

### 📌 Task 1: Running MySQL Container with Persistent Storage

#### 🔹 Subtask 1.1: Create Persistent Volume Directory
Generate a standard local tracking host directory to store relational database assets:
```bash
mkdir -p mysql-data
```
💡 **Key Concept:** This host folder maps directly over the containerized engine path to ensure your transactional database records persist outside transient image layers.

#### 🔹 Subtask 1.2: Launch the MySQL Container Engine
Initialize an isolated background instance utilizing explicit host mapping structures and seed parameter environments:

```bash
podman run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=redhat123 \
  -e MYSQL_DATABASE=testdb \
  -e MYSQL_USER=testuser \
  -e MYSQL_PASSWORD=user123 \
  -v \$(pwd)/mysql-data:/var/lib/mysql \
  -p 3306:3306 \
  docker.io/library/mysql:8.0
```

💡 **Key Parameters:**
* `-v $(pwd)/mysql-data:/var/lib/mysql`: Binds your host directory to the internal data collection path of the database.
* `-e ENV_VARS`: Configures the initial logical user schema layouts, database naming fields, and runtime root passwords.

**Verification:** Check engine tracking metrics and running statuses:
```bash
podman ps -a --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"
```
🏁 **Expected Output:** Shows the `mysql-db` workload instance as actively running.

#### 🔹 Subtask 1.3: Test Database Connection and Seed Rows
Log inside your transactional sandbox boundary line to populate raw schema targets:
```bash
podman exec -it mysql-db mysql -u testuser -puser123 testdb
```

Once inside the interactive `mysql>` command prompt wrapper shell, populate your lab tables:
```sql
CREATE TABLE lab_data (id INT AUTO_INCREMENT PRIMARY KEY, message VARCHAR(255));
INSERT INTO lab_data (message) VALUES ('Persistent test data');
SELECT * FROM lab_data;
exit;
```
🏁 **Expected Output:** Shows a single database table containing your inserted test record string row layout.

---

### 📌 Task 2: Verify Data Persistence Across Destruction

#### 🔹 Subtask 2.1: Purge the Workload Container Thread
Simulate an upgrade or server disaster scenario by force-clearing your workload instance:
```bash
podman stop mysql-db
podman rm mysql-db
```

#### 🔹 Subtask 2.2: Recreate a Container Pointing to the Existing Path
Spin up a completely blank database engine container, attaching it back to the exact same host directory storage layer context:
```bash
podman run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=redhat123 \
  -v \$(pwd)/mysql-data:/var/lib/mysql \
  -p 3306:3306 \
  docker.io/library/mysql:8.0
```

#### 🔹 Subtask 2.3: Verify Application State Preservation
Instruct the fresh engine initialization routine to query historical records to verify survival:
```bash
podman exec -it mysql-db mysql -u testuser -puser123 testdb -e "SELECT * FROM lab_data;"
```
🏁 **Expected Output:** The query pulls back your historical tracking information row exactly as before, proving persistence.

---

### 📌 Task 3: PostgreSQL Implementation (Optional Challenge)

#### 🔹 Subtask 3.1: Create PostgreSQL Volume Directory
Set up a clean host folder structure to map your alternate engine path targets:
```bash
mkdir -p pg-data
```

#### 🔹 Subtask 3.2: Launch the PostgreSQL Container Instance
Initialize the isolated Postgres cluster database workload path:
```bash
podman run -d \
  --name postgres-db \
  -e POSTGRES_PASSWORD=redhat123 \
  -e POSTGRES_USER=testuser \
  -e POSTGRES_DB=testdb \
  -v \$(pwd)/pg-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  docker.io/library/postgres:13
```

#### 🔹 Subtask 3.3: Seed Data & Execute Lifecycle Hardening Checks
Populate target tables and test transactional validation scripts using `psql`:
```bash
podman exec -it postgres-db psql -U testuser -d testdb -c "CREATE TABLE lab_data (id SERIAL PRIMARY KEY, message TEXT); INSERT INTO lab_data (message) VALUES ('Postgres persistent data');"
```

*🔥 Challenge Step:* To finish this optional challenge, follow the container teardown and reconstitution routines from Task 2 to confirm data survival under the PostgreSQL ecosystem.

---

## 🛠️ Troubleshooting Tips
* ❌ **Database Crash loop / Access Errors?** Container user namespaces may lack write permissions for host directories. Adjust local user rights layout manually if necessary:
  ```bash
  sudo chown -R 1001:1001 mysql-data/
  ```
* ❌ **Port Conflicts (`Address already in use`)?** Verify that a native database service isn't already occupying target sockets before container launch:
  ```bash
  ss -tulnp | grep -E "3306|5432"
  ```
* ❌ **Engine initialization failures?** Fetch the raw process output context to trace logical environment parsing issues:
  ```bash
  podman logs mysql-db
  ```

---

## 🎉 Conclusion & Review
In this lab scenario, you successfully achieved the following:
* **Deployed** stateful, database-driven container applications using persistent volumes.
* **Mapped** runtime transactions to host directories via explicit path bindings.
* **Verified** total application layer configuration integrity across destructive engine lifecycles.

### 🧠 Knowledge Check
1. **What happens if you omit the `-v` volume flag when running a database container?**
   * *Answer:* Storage remains ephemeral. When the container process terminates or gets removed, your databases, schemas, and tracking history are lost forever.
2. **How would you migrate this persistent data to another host?**
   * *Answer:* Since files reside directly on the host file structure, you can stop the engine, compress the storage folder into an archive (`tar -czf db_backup.tar.gz ./mysql-data`), move the file to a new host, and unpack it beneath the fresh container mount path.
3. **What security considerations apply to storing database files on the host?**
   * *Answer:* Host folder file access maps transparently to standard users. Unprivileged processes on the host can manipulate base database source files if access control lists or SELinux policies are not properly enforced.

---

## 🚀 Further Exploration
* Transition configurations from explicit path directory mounts to managed abstractions via `podman volume create`.
* Test advanced multi-container network deployments utilizing **Podman Compose** structures.
* Implement structured live snapshot backups of directory states while database container engines are actively processing threads.
