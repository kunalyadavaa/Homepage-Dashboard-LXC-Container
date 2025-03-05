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

Let me know if you need more help! 🚀