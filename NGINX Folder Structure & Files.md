## 2. NGINX Folder Structure & Files — Simple, step-by-step for a layman


---

### 🔧 Quick map (what you already wrote)
/etc/nginx/nginx.conf – Main config  
/etc/nginx/sites-available/  
/etc/nginx/sites-enabled/  
/var/www/html/  
/etc/nginx/conf.d/*.conf  

Log files:  
- access.log  
- error.log  

---

## 1) `/etc/nginx/nginx.conf` — the master configuration file

### **What it is:**  
Think of this as NGINX’s **“control panel”** file. It contains global settings (how many worker processes to use, which extra config files to include, global timeouts, etc.). NGINX reads this file when it starts.

### **Why it matters:**  
If something needs to be changed for the whole server (e.g., enable gzip compression, set number of workers), you usually do it here or include files from here.

### **How it looks (simple example):**
```nginx
user www-data;
worker_processes auto;

events {
  worker_connections 1024;
}

http {
  include       /etc/nginx/mime.types;
  default_type  application/octet-stream;

  sendfile on;
  keepalive_timeout 65;

  include /etc/nginx/conf.d/*.conf;
  include /etc/nginx/sites-enabled/*;
}
```

### **Commands to inspect:**
```bash
sudo cat /etc/nginx/nginx.conf
sudo less /etc/nginx/nginx.conf
```

---

## 2) `/etc/nginx/sites-available/` — place to keep site configs

### **What it is:**  
This folder usually contains configuration files for each website (each “site” or domain). Example: `example.com.conf`.

### **Why it matters:**  
You store your **site-specific settings** here:
- domain names  
- document root  
- SSL configuration  
- redirects  

Files here **are NOT active** unless linked to `sites-enabled`.

### **Typical file name:**  
`yourdomain.conf` or `example.com`

### **Example server block:**
```nginx
server {
  listen 80;
  server_name example.com www.example.com;
  root /var/www/example.com;
  index index.html index.htm;
}
```

---

## 3) `/etc/nginx/sites-enabled/` — what NGINX actually uses

### **What it is:**  
Contains **active** site config files.  
On Ubuntu/Debian, you enable a site by creating a *symbolic link* from `sites-available`.

### **How enabling works:**
```bash
# create the config in sites-available
sudo nano /etc/nginx/sites-available/example.com

# enable it by creating a symlink in sites-enabled
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/

# test and reload nginx
sudo nginx -t
sudo systemctl reload nginx
```

### **Note:**  
On some distros (CentOS/RHEL), `sites-available` and `sites-enabled` **do not exist**.  
They use `/etc/nginx/conf.d/` instead.

---

## 4) `/etc/nginx/conf.d/*.conf` — extra configs included by nginx.conf

### **What it is:**  
Files inside `conf.d` are automatically loaded by:

```
include /etc/nginx/conf.d/*.conf;
```

### **Common uses:**
- global config snippets  
- small site configs  
- third-party packages  

### **Naming rule:**  
Files **must end with `.conf`**.

---

## 5) `/var/www/html/` — default web root (where files are served from)

### **What it is:**  
This is the **default website folder**.  
When someone visits:  
`http://server-ip/`  
NGINX loads `index.html` from here (unless changed).

### **Example usage:**
```bash
sudo nano /var/www/html/index.html
```
Visit:  
`http://your-server-ip/`

### **Permissions:**
Files must be readable by NGINX (usually user `www-data` or `nginx`):

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

## 6) Log files — `access.log` and `error.log`

### **Locations:**
- `/var/log/nginx/access.log` — every request  
- `/var/log/nginx/error.log` — warnings & errors  

### **Why they matter:**
- `access.log` → traffic, who visited, status codes  
- `error.log` → helps debug problems  

### **Watch logs live:**
```bash
sudo tail -f /var/log/nginx/access.log /var/log/nginx/error.log
```

### **Tip:**  
Logs rotate automatically so they don’t grow forever.

---

## ⭐ Extra Helpful Notes (Beginner-friendly)

### ✅ **Testing config before reload**
```bash
sudo nginx -t
sudo systemctl reload nginx
```

### ✅ **Who runs NGINX?**
Check `user` in:
```
/etc/nginx/nginx.conf
```

### ✅ **403 Forbidden fix (permissions problem)**
```bash
sudo chown -R www-data:www-data /var/www/your-site
sudo find /var/www/your-site -type d -exec chmod 755 {} \;
sudo find /var/www/your-site -type f -exec chmod 644 {} \;
```

### ✅ **SELinux issues (CentOS/RHEL)**
```bash
sudo semanage fcontext -a -t httpd_sys_content_t "/var/www(/.*)?"
sudo restorecon -Rv /var/www
```

### ✅ **Differences between distributions**
- **Debian/Ubuntu:** uses `sites-available` + `sites-enabled`  
- **CentOS/RHEL:** uses only `conf.d/`  

---

## 📌 Small Checklist to Explore on Your Server

```bash
# check main config
sudo nginx -t
sudo less /etc/nginx/nginx.conf

# list site configs
ls -la /etc/nginx/sites-available
ls -la /etc/nginx/sites-enabled

# see conf.d files
ls -la /etc/nginx/conf.d

# check web root
ls -la /var/www/html
sudo cat /var/www/html/index.html

# view logs
sudo tail -n 50 /var/log/nginx/access.log
sudo tail -n 50 /var/log/nginx/error.log
```

---

## ✅ Final Plain-English Summary

- **`nginx.conf`** → main control file  
- **`sites-available`** → site configs (drafts)  
- **`sites-enabled`** → active sites (symlinks)  
- **`conf.d`** → additional configs  
- **`/var/www/html`** → your actual website files  
- **access.log / error.log** → who visited & what went wrong  

