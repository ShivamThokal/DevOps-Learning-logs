## 1. SSH configuration on Ubuntu

**Goal:** Understand the SSH server settings that control root login and password authentication.

### Step 1: Open the SSH configuration directory

```bash
cd /etc/ssh/
```

### Step 2: Edit the main SSH configuration

```bash
sudo nano /etc/ssh/sshd_config
```

Find or add these settings:

```text
PermitRootLogin yes
PasswordAuthentication yes
```

Remove the `#` at the beginning if the settings are commented out.

### Step 3: Check the SSH drop-in configuration

```bash
cd /etc/ssh/sshd_config.d/
ls
```

A file may be named `60-cloudimg-settings.conf` or have a different name.

Open the relevant file:

```bash
sudo nano 60-cloudimg-settings.conf
```

Set:

```text
PasswordAuthentication yes
```

If a different cloud-init configuration file exists, inspect that file instead. SSH configuration drop-ins can override or affect settings in the main file.

### Step 4: Validate and restart SSH

Keep your current SSH session open while testing. First validate the configuration:

```bash
sudo sshd -t
```

No output generally means the syntax check passed.

Check the effective settings:

```bash
sudo sshd -T | grep -E 'permitrootlogin|passwordauthentication'
```

If the configuration is valid, restart SSH:

```bash
sudo systemctl restart ssh
```

On some distributions, the service may also be addressed as `sshd`:

```bash
sudo systemctl restart sshd
```

### Important security note

The settings `PermitRootLogin yes` and `PasswordAuthentication yes` allow direct root login and password-based SSH authentication. **Do not expose this configuration to the public internet for production use.**

Safer approach:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Use a normal user with `sudo` privileges and SSH key authentication. If password login is required temporarily for a lab, restrict SSH access in the cloud Security Group to your own IP address (TCP port 22), use a strong password, and revert the setting afterwards. Never close your working SSH session until a second login has been tested successfully.

---

## 2. Web servers / application servers

| Name | Purpose |
|---|---|
| **Nginx** | Web server, reverse proxy and load balancer |
| **Apache HTTP Server** | Web server; package/service name is `httpd` on Amazon Linux and `apache2` on Ubuntu |
| **Tomcat** | Java application server / Servlet container; runs Java web applications |

### What is a reverse proxy?

A **reverse proxy** receives requests from clients and forwards them to backend application servers. It can also handle SSL/TLS termination, load balancing, caching and routing.

Example:

```text
Client / Browser
       |
       v
  Nginx (Reverse Proxy)
       |
       v
 Backend Application
 (Node.js / Tomcat / Python)
```

---

## 3. Identify your Linux distribution

Before installing packages, check the operating system:

```bash
cat /etc/os-release
```

This guide uses **Amazon Linux 2023** and **Ubuntu 26.10 / 26.04 LTS**. Amazon Linux 2023 uses `dnf`; Ubuntu uses `apt`.

## 4. Install Apache HTTP Server on Amazon Linux 2023

On Amazon Linux, Apache's package and service name is `httpd`.

### Step 1: Update packages

```bash
sudo dnf update -y
```

### Step 2: Install Apache

```bash
sudo dnf install httpd -y
```

### Step 3: Start and enable Apache

```bash
sudo systemctl enable --now httpd
```

### Step 4: Verify

```bash
sudo systemctl status httpd
httpd -v
```

Look for `active (running)` in the service status.

## 5. Install Nginx on Amazon Linux 2023

### Step 1: Update packages

```bash
sudo dnf update -y
```

### Step 2: Install Nginx

```bash
sudo dnf install nginx -y
```

### Step 3: Start and enable Nginx

```bash
sudo systemctl enable --now nginx
```

### Step 4: Verify

```bash
sudo systemctl status nginx
nginx -v
```

## 6. Install Apache (`apache2`) on Ubuntu

### Step 1: Update package lists

```bash
sudo apt update
```

### Step 2: Install Apache

```bash
sudo apt install apache2 -y
```

### Step 3: Start and enable Apache

```bash
sudo systemctl enable --now apache2
```

### Step 4: Verify

```bash
sudo systemctl status apache2
apache2 -v
```

## 7. Install Nginx on Ubuntu

### Step 1: Update package lists

```bash
sudo apt update
```

### Step 2: Install Nginx

```bash
sudo apt install nginx -y
```

### Step 3: Start and enable Nginx

```bash
sudo systemctl enable --now nginx
```

### Step 4: Verify

```bash
sudo systemctl status nginx
nginx -v
```

## 8. `httpd` vs `apache2` — remember this

| Operating system | Apache package | Apache service |
|---|---|---|
| Amazon Linux 2023 | `httpd` | `httpd` |
| Ubuntu | `apache2` | `apache2` |

`httpd` and `apache2` are names for the same Apache HTTP Server software on different Linux distributions. On Ubuntu, install and manage `apache2`; do not try to install `httpd` as a separate web server.

## 9. Test the web server

### Step 1: Test locally from the instance

```bash
curl -I http://localhost
```

A successful response commonly includes:

```text
HTTP/1.1 200 OK
```

### Step 2: Allow HTTP in AWS

In the EC2 Security Group, add an inbound rule:

| Type | Protocol | Port | Source |
|---|---|---:|---|
| HTTP | TCP | 80 | Your IP or an appropriate allowed range |

For a public website, `0.0.0.0/0` allows access from anywhere over IPv4. Use this only when the website is intended to be public. Also check the instance firewall if one is enabled.

### Step 3: Open the website

Find the instance's Public IPv4 address and visit:

```text
http://YOUR_PUBLIC_IP
```

### Step 4: Add a simple test page

The common Apache document root is `/var/www/html`. Nginx's default document root can vary by distribution/package; check its active server configuration.

For a simple test on either server:

```bash
echo "<h1>Hello from Linux Web Server</h1>" | sudo tee /var/www/html/index.html
```

Refresh the browser. If the Nginx default site uses a different root, put the test page in that configured directory instead.

## 10. Basic service commands

| Action | Amazon Linux Apache | Ubuntu Apache | Nginx (both) |
|---|---|---|---|
| Start | `sudo systemctl start httpd` | `sudo systemctl start apache2` | `sudo systemctl start nginx` |
| Stop | `sudo systemctl stop httpd` | `sudo systemctl stop apache2` | `sudo systemctl stop nginx` |
| Restart | `sudo systemctl restart httpd` | `sudo systemctl restart apache2` | `sudo systemctl restart nginx` |
| Enable at boot | `sudo systemctl enable httpd` | `sudo systemctl enable apache2` | `sudo systemctl enable nginx` |
| Status | `sudo systemctl status httpd` | `sudo systemctl status apache2` | `sudo systemctl status nginx` |

**Note:** Apache and Nginx commonly both listen on port 80 by default. Stop one before starting the other with the default configuration, or configure different ports / a reverse-proxy setup.

## 11. Troubleshooting commands

Check which process is using port 80:

```bash
sudo ss -tulpn | grep ':80'
```

Test Nginx configuration:

```bash
sudo nginx -t
```

Test Apache configuration on Amazon Linux:

```bash
sudo httpd -t
```

Test Apache configuration on Ubuntu:

```bash
sudo apache2ctl configtest
```

View service logs (replace the service name as appropriate):

```bash
sudo journalctl -u nginx
sudo journalctl -u httpd
sudo journalctl -u apache2
```

## 12. Quick installation revision

| Server | Amazon Linux 2023 | Ubuntu |
|---|---|---|
| Apache | `sudo dnf install httpd -y` | `sudo apt install apache2 -y` |
| Nginx | `sudo dnf install nginx -y` | `sudo apt install nginx -y` |

**Ports to remember:**

- SSH: TCP `22`
- HTTP: TCP `80`
- HTTPS: TCP `443`

---

## Servers and Clients Work Together

![How Servers and Clients Work Together](\images\How_Servers_and_Clients_Work_Together.png)


---

**Dated:** 20 November 2026
