This is a great exercise for understanding how networking works "under the hood" in a Linux environment. We will stick to the modern `ip` and `ss` tools.

### Phase 1: The Investigation (Commands & Execution)

#### 1. Identify your Internal IP

Instead of the old `ifconfig`, we use the **ip route2** suite.

```bash
ip addr show

```

* **What to look for:** Look for an interface named something like `eth0` or `enp0s3`. The internal IP is the number following `inet` (e.g., `192.168.1.15`).
* **Why?** This is the address other machines on your local network use to find this specific VM.

#### 2. Check the "Listeners"

To see what services are waiting for connections, we use `ss` (socket statistics).

```bash
sudo ss -tunlp | grep :22

```

* **Command Breakdown:** * `-t` (TCP), `-u` (UDP), `-n` (numeric ports), `-l` (listening), `-p` (show process/PID).
* **Expected Output:** `LISTEN  0  128  0.0.0.0:22  0.0.0.0:* users:(("sshd",pid=842,fd=3))`
* **The Answer:** In this example, the **PID** listening on port 22 is **842** (belonging to the `sshd` service).

#### 3. Local Connectivity Test

We use `curl` to talk to the web server locally.

```bash
curl http://localhost:80

```

* **If it fails:** You will likely see: `curl: (7) Failed to connect to localhost port 80: Connection refused`.
* **The Error Code (7):** This means the "plumbing" is broken. Specifically, there is no service (like Apache or Nginx) actually listening on port 80, or a firewall is blocking the attempt immediately.

---

### Phase 2: The Core Concepts

#### 1. Public IP vs. Private IP & NAT

* **Private IP:** This is like an internal extension in an office building. It only works inside your specific network (VPC). It’s free and secure because it isn't reachable from the outside world.
* **Public IP:** This is like a global phone number. It is unique across the entire internet.
* **Why NAT?** We use **Network Address Translation** because the world ran out of IPv4 addresses. NAT allows a whole "building" of private IPs to share one single Public IP to reach the internet. It also acts as a security barrier, preventing outsiders from initiating a direct connection to your internal VM.

#### 2. The "Loopback" Address

* **What it is:** Think of the loopback as a "virtual mirror." It allows a computer to talk to itself. When you send data to the loopback, it never leaves the network card; it loops right back into the system.
* **The IP:** The standard loopback IP is **127.0.0.1**.

---

### Summary of Commands Used

| Task | Command |
| --- | --- |
| **Check IP** | `ip addr` |
| **Check Listeners** | `sudo ss -tunlp` |
| **Local Web Test** | `curl -I localhost:80` |
| **Verify Port 22** | `sudo ss -lp |

Would you like me to show you how to install a web server like **Nginx** so that the `curl` command actually succeeds?
