#### Homepage dashboard in Proxmox LXC container

If your **Homepage dashboard** is running inside a **Proxmox LXC container**, follow these steps to access its terminal:

---

### **1. Login via Proxmox Web UI**
1. Open your browser and go to:
   ```
   https://<your-proxmox-ip>:8006
   ```
2. Log in with your **Proxmox credentials**.
3. In the **left sidebar**, expand your node (e.g., `pve`).
4. Click on the **LXC container** running your Homepage dashboard.
5. Click **Console** to open the terminal.

---

### **2. Login via Proxmox Shell**
If you are logged into **Proxmox** via SSH or the Web UI shell, you can attach to the container manually:

1. Find the container ID by running:
   ```bash
   pct list
   ```
2. Attach to the container shell:
   ```bash
   pct enter <container-id>
   ```
   (Replace `<container-id>` with the actual container ID from step 1.)

3. Now, you are inside the LXC container and can run commands.

---

### **3. Exit the Container**
To leave the container shell, type:
```bash
exit
```
It looks like you're trying to SSH into your **Proxmox LXC container** as **root**, but the password is being rejected. Here’s how to fix it:

---
#### VScode SSH

### **1. Ensure the Root Password is Set in the LXC Container**
1. Log in to the **Proxmox Web UI** (`https://<proxmox-ip>:8006`).
2. Open the **Console** of the LXC container.
3. Run:
   ```bash
   passwd root
   ```
   Set a new password for root.

---

### **2. Allow Root SSH Login in the LXC Container**
1. Inside the container, edit the SSH config file:
   ```bash
   nano /etc/ssh/sshd_config
   ```
2. Look for this line:
   ```
   PermitRootLogin prohibit-password
   ```
   Change it to:
   ```
   PermitRootLogin yes
   ```
3. Save the file (`CTRL+X`, then `Y`, then `Enter`).

4. Restart SSH:
   ```bash
   systemctl restart ssh
   ```

---

### **3. Check If the LXC Container Allows SSH**
Some **unprivileged** LXC containers may block SSH access.  
To fix this, run the following on the **Proxmox host** (not inside the container):

```bash
pct exec <container-id> -- systemctl start ssh
```

To enable SSH on boot:
```bash
pct exec <container-id> -- systemctl enable ssh
```

---

### **4. Try SSH Again**
Now, from your local machine, try:
```bash
ssh root@192.168.29.176
```
Enter the **newly set password**.

---

### **5. If You Still Get "Permission Denied"**
Check for **failed SSH attempts** in the container:
```bash
journalctl -xe | grep ssh
```

If the issue persists, **disable PasswordAuthentication** and use SSH keys instead:
1. On your **local machine**, generate SSH keys:
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
2. Copy the public key to the container:
   ```bash
   ssh-copy-id root@192.168.29.176
   ```
3. Try SSH login again.

---

Let me know if you're still stuck! 🚀
