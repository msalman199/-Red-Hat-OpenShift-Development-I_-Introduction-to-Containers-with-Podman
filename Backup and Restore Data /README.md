# 💾 Backup and Restore Data 
## 🎯 Objectives
* **Implement** secure database backup strategies for decoupled, stateful containers.
* **Perform** localized logical database dumps directly from running container runtimes.
* **Store** extracted asset files inside infrastructure-independent container volumes.
* **Restore** core schemas and row entries into fresh destination database targets.

---

## 📋 Prerequisites & Setup

### 🔧 Minimum System Requirements
* Standard console terminal or virtual machine command-line access.
* Internet availability to pull base database and helper distribution images.
* **Podman Engine (v3.0+)** or **Docker** installed and ready on the system.

### 🛠️ Workspace Preparation & Verification

1. **Verify your engine version metrics:**
   ```bash
   podman --version
   ```

2. **Generate and navigate into your dedicated lab repository workspace:**
   ```bash
   mkdir -p ~/backup-lab && cd ~/backup-lab
   ```

---

## 🧪 Lab Execution Steps

### 📌 Task 1: Perform Database Dumps Inside Container

#### 🔹 Subtask 1.1: Initialize a Source MySQL Instance
Spin up an isolated source relational database instance running in the background:
```bash
podman run -d --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=redhat \
  -e MYSQL_DATABASE=testdb \
  -e MYSQL_USER=testuser \
  -e MYSQL_PASSWORD=testpass \
  docker.io/library/mysql:8.0
```

**Verification:** Check to make sure the target worker node is up and responsive:
```bash
podman ps -f name=mysql-db
```

#### 🔹 Subtask 1.2: Seed Sample Operational Schema Data
Access the background database context using the embedded local CLI binary tool:
```bash
podman exec -it mysql-db mysql -u root -predhat
```

Once inside the interactive `mysql>` command execution interface, seed a testing table structure:
```sql
USE testdb;
CREATE TABLE employees (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50));
INSERT INTO employees (name) VALUES ('John Doe'), ('Jane Smith');
SELECT * FROM employees;
exit
```

#### 🔹 Subtask 1.3: Generate a Structured Database Dump
Stream the internal state out of the engine using `mysqldump` and catch the file stream directly onto your host workstation storage layer:
```bash
podman exec mysql-db /usr/bin/mysqldump -u root -predhat testdb > testdb_dump.sql
```

**Verification:** Confirm that the flat text file configuration exists and contains logical database operations:
```bash
ls -l testdb_dump.sql
head -n 5 testdb_dump.sql
```

---

### 📌 Task 2: Store Dumps on Managed Volumes

#### 🔹 Subtask 2.1: Provision a Persistent Storage Volume
Create an infrastructure-managed asset storage area that persists independently of your runtime containers:
```bash
podman volume create backup-vol
```

#### 🔹 Subtask 2.2: Stage the Backup File Inside the Volume
Initialize a short-lived Alpine Linux context container to structure directories directly within the volume:
```bash
podman run -it --rm -v backup-vol:/backup alpine sh -c "mkdir -p /backup/mysql"
```

Safely pass your file structure down into the dedicated backing volume utilizing an immutable layer creation method:
```bash
podman cp testdb_dump.sql $(podman create --name temp -v backup-vol:/backup alpine):/backup/mysql/
podman rm temp
```

**Verification:** Confirm that your backup file is correctly written into the managed storage layer:
```bash
podman run --rm -v backup-vol:/backup alpine ls -l /backup/mysql
```

---

### 📌 Task 3: Restore Data from Dumps

#### 🔹 Subtask 3.1: Create a Clean Database Destination
Spin up a completely fresh database service instance representing a clean target environment:
```bash
podman run -d --name mysql-restore \
  -e MYSQL_ROOT_PASSWORD=redhat \
  -e MYSQL_DATABASE=testdb \
  -e MYSQL_USER=testuser \
  -e MYSQL_PASSWORD=testpass \
  docker.io/library/mysql:8.0
```

#### 🔹 Subtask 3.2: Pull and Import the Database Dump
Retrieve the target asset text configuration straight from your managed isolation tracking volume:
```bash
podman cp $(podman create --name temp -v backup-vol:/backup alpine):/backup/mysql/testdb_dump.sql .
podman rm temp
```

Pipe the database instructions straight back into the input layer stream of your newly initialized recovery instance:
```bash
podman exec -i mysql-restore mysql -u root -predhat testdb < testdb_dump.sql
```

#### 🔹 Subtask 3.3: Verify Final Schema Restoration Success
Query the recovered database instance directly from your terminal to verify row data integration:
```bash
podman exec -it mysql-restore mysql -u root -predhat -e "SELECT * FROM testdb.employees;"
```
🏁 **Expected Output:**
```text
+----+------------+

| id | name       |
+----+------------+

|  1 | John Doe   |
|  2 | Jane Smith |
+----+------------+
```

---

## 🛠️ Troubleshooting Tips
* ❌ **MySQL Container Fails to Start?** Trace initialization and runtime environmental logging scripts directly:
  ```bash
  podman logs mysql-db
  ```
* ❌ **Permission Denied / Storage Issues?** If storage mount structures cause mapping faults, apply explicit SELinux namespace label configurations:
  ```bash
  -v backup-vol:/backup:Z
  ```
* ❌ **Dump Errors or Missing Tables?** Double-check password declarations, syntax spaces, and database naming cases to match your initial parameters exactly.

---

## 🎉 Conclusion
In this scenario deployment lab, you mastered the following:
* **Deployed** active MySQL services inside container topologies.
* **Extracted** database states smoothly utilizing structured `mysqldump` parameter pipelines.
* **Secured** business files inside lifecycle-decoupled, managed persistent tracking volumes.
* **Restored** system states back into clean destination servers without breaking runtime code constraints.

*These tasks simulate the operational patterns essential for maintaining production database workloads in **Red Hat OpenShift** and Kubernetes enterprise cloud environments.*

---

## 🧹 Workspace Cleanup
Run the following commands to safely purge your test environments, wipe matching allocations, and release local storage:

```bash
podman stop mysql-db mysql-restore
podman rm mysql-db mysql-restore
podman volume rm backup-vol
rm -f testdb_dump.sql
```
