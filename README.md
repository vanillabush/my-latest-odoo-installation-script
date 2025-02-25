
```markdown
# Odoo 18 Installation Script

This script automates the installation of Odoo 18 on a Linux server. It covers everything from system preparation to post-installation steps.

## Prerequisites

Ensure you have the following installed:
- A Linux server (Ubuntu 22.04 LTS recommended)
- SSH access
- `sudo` privileges

## Steps to Install Odoo 18

### Step 1: Prepare Your Linux System

**Access Your Server**:
- Log in to your Linux server via SSH or directly if you're using a local machine.

**Update the System**:
- Ensure your system is up to date:
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```

**Install Git (if not already installed)**:
- Git is required to clone the Odoo source code.
  ```bash
  sudo apt install git -y
  ```

### Step 2: Download and Run the Script

**Download the Script**:
- Save your script to a file (e.g., `install_odoo18.sh`):
  ```bash
  nano install_odoo18.sh
  ```
- Paste the script content into the file and save it (Ctrl + O, then Ctrl + X).

**Make the Script Executable**:
- Grant execute permissions to the script:
  ```bash
  chmod +x install_odoo18.sh
  ```

**Run the Script**:
- Execute the script with `sudo`:
  ```bash
  sudo ./install_odoo18.sh
  ```

### Step 3: Monitor the Installation
The script will:
- Install Python 3.11.
- Install PostgreSQL (version 14 or default, depending on your configuration).
- Create a system user (`odoo`).
- Set up the Odoo directory structure in `/opt/odoo-18`.
- Clone the Odoo 18 source code.
- Create a Python virtual environment and install dependencies.
- Generate an Odoo configuration file (`odoo.conf`).
- Create and enable a `systemd` service (`odoo-18.service`).

**Watch for Errors**:
- If any step fails, the script will stop, and you’ll see an error message. Common issues include:
  - Missing dependencies.
  - Permission issues (ensure you’re running the script with `sudo`).
  - Network issues (e.g., GitHub or package repository unreachable).

### Step 4: Verify the Installation

**Check Odoo Service Status**:
- After the script completes, verify the Odoo service is running:
  ```bash
  sudo systemctl status odoo-18
  ```
- Look for `active (running)` in the output.

**Access Odoo in Your Browser**:
- Open your browser and navigate to:
  ```
  http://<your-server-ip>:8071
  ```
- Replace `<your-server-ip>` with your server's IP address or `localhost` if running locally.

**Check Logs**:
- If Odoo doesn’t start, check the logs for errors:
  ```bash
  journalctl -u odoo-18 -n 100 --no-pager
  ```

### Step 5: Post-Installation Steps

**Secure Odoo**:
- Change the default `admin_passwd` in `/opt/odoo-18/config/odoo.conf` to a strong password.

**Set Up a Reverse Proxy (Optional)**:
- If you installed Nginx (`INSTALL_NGINX="True"`), configure it as a reverse proxy for Odoo.

**Enable SSL (Optional)**:
- If you enabled SSL (`ENABLE_SSL="True"`), ensure you’ve configured your domain and SSL certificates (e.g., using Let’s Encrypt).

**Add Custom Modules**:
- Place your custom modules in `/opt/odoo-18/custom/addons` and update the `addons_path` in `odoo.conf` if needed.

### Step 6: Manage the Odoo Service

**Start/Stop/Restart Odoo**:
```bash
sudo systemctl start odoo-18
sudo systemctl stop odoo-18
sudo systemctl restart odoo-18
```

**Enable/Disable Auto-Start**:
```bash
sudo systemctl enable odoo-18   # Enable auto-start on boot
sudo systemctl disable odoo-18  # Disable auto-start on boot
```

### Step 7: Test Server Reboot

**Reboot your server to ensure Odoo starts automatically**:
```bash
sudo reboot
```

**After the server restarts, check the Odoo service status**:
```bash
sudo systemctl status odoo-18
```

### Troubleshooting

**Odoo Fails to Start**:
- Check logs:
  ```bash
  journalctl -u odoo-18 -n 100 --no-pager
  ```
- Verify paths in `/etc/systemd/system/odoo-18.service` and `/opt/odoo-18/config/odoo.conf`.

**Port Conflict**:
- Ensure no other service is using port 8071. Change the port in `odoo.conf` if needed.

**PostgreSQL Issues**:
- Ensure PostgreSQL is running and accessible by the `odoo` user.

By following these steps, you should have a fully functional Odoo 18 installation on your Linux server. Let me know if you encounter any issues!
```

