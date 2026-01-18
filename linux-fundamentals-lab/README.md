# 🚀 DevOps Journey: Linux & System Fundamentals

This repository documents my daily progress, lab implementations, and technical evaluations as I transition into a Cloud DevOps Engineer role.

---

## 📅 Day 1: The Linux Filesystem & Permissions

### 📖 Overview
In DevOps, "Everything is a file." If you don't understand permissions and ownership, you will constantly break your CI/CD pipelines. Today’s focus was on environment parity, the "Least Privilege" security model, and process lifecycle management.

---

### 🧪 Tasks & Lab Results

#### 1. Environment Setup
* **Action:** Installed **VMware and setup a Alma linux env** on Windows.
* **Purpose:** To ensure a local development environment that matches the Linux kernels used in production cloud servers (AWS/Azure/GCP).

#### 2. Permission Mastery
* **Task:** Create `deploy.sh` and restrict access so only the owner can read/write/execute.
* **Implementation:**
    ```bash
    touch deploy.sh
    chmod 700 deploy.sh
    ```
* **Verification (The Security Test):** I simulated an unauthorized user to verify the restriction:
    ```bash
    sudo -u nobody cat deploy.sh
    # Result: Permission denied
    ```


#### 3. Process Inspection
* **Task:** Identify and manage a background process.
* **Implementation:** 1. Started a background task: `sleep 1000 &`
    2. Located the PID: `ps aux | grep sleep`
    3. Monitored via `top` (using `o` filter for `COMMAND=sleep`).
* **Termination Protocol:**
    * **Graceful:** `kill <PID>` (SIGTERM) - Allows cleanup.
    * **Forceful:** `kill -9 <PID>` (SIGKILL) - Immediate termination (Emergency only).

---

### 🧠 Concept Check: The First Hurdle

#### **Q: What is the difference between `sudo su -` and `sudo <command>`?**
* **`sudo <command>`**: Borrowing root power for one task while keeping your own user environment.
* **`sudo su -`**: Spawning a **login shell**. This loads the root user's profile, variables, and path.

#### **Q: What is `/etc/sudoers` and why use `visudo`?**
* The `/etc/sudoers` file defines who has administrative power. 
* **NEVER** edit it with nano/vi. Use **`visudo`**. 
* `visudo` checks for syntax errors before saving. A typo in this file can lock you out of your server permanently.

---

### 📈 Technical Review & Evaluation
**Current Rating: 7/10**

> **Lead Engineer Feedback:**
> * **Correction:** Initially struggled to find quiet processes in `top`. 
> * **Optimization:** Learned that `pgrep` or `ps` is more efficient for finding specific IDs, while `top` is better for monitoring high-resource "hogs."
> * **Safety Note:** Never use `kill -9` as a first option in production; it risks data corruption. Use SIGTERM first.

- [x] Linux File Permissions (Numerical Method)
- [x] Process Lifecycle (SIGTERM vs SIGKILL)
- [x] Secure Sudo Administration (`visudo`)
