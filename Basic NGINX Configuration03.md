## 3. Basic NGINX Configuration
Understanding the four main blocks inside `nginx.conf`:
- **events**
- **http**
- **server**
- **location**

---

### 🔹 1) `events { }` Block  
Controls how NGINX handles **network connections**.

```nginx
events {
    worker_connections 1024;
}
```

- Defines how many users/connections can be handled at once.
- Applies globally.

---

### 🔹 2) `http { }` Block  
This is the main block that contains **web traffic settings**.

```nginx
http {
    include mime.types;
    sendfile on;
    keepalive_timeout 65;
}
```

Used for:
- gzip
- logging
- mime types
- caching
- server blocks
- timeouts

---

### 🔹 3) `server { }` Block  
Defines **one domain or website**.

```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;
}
```

Each `server` block = one hosted website.

---

### 🔹 4) `location { }` Block  
Controls how NGINX handles specific **URL paths**.

```nginx
location /images/ {
    alias /var/www/images/;
}
```

Used for:
- static files  
- routing  
- API endpoints  
- reverse proxy paths  

---

### 📘 Diagram (Understanding the Structure)

```
NGINX CONFIG
│
├── events {}        ← Handles connections
│
└── http {}          ← Handles web traffic
      │
      ├── server {}      ← One website
      │       │
      │       └── location {}  ← URL-specific rules
      │
      └── server {}      ← Another website
```

---


## 🔧 Starting, Stopping, Restarting NGINX

NGINX is controlled using `systemctl` on most modern Linux systems.

### ▶️ Start NGINX
Starts the NGINX service.
```bash
sudo systemctl start nginx
```

---

### ⏹ Stop NGINX
Stops the NGINX service completely.
```bash
sudo systemctl stop nginx
```

---

### 🔄 Restart NGINX
Stops NGINX and starts it again.
Use this when you make major configuration changes.
```bash
sudo systemctl restart nginx
```

---

### 🔁 Reload NGINX (Recommended)
Reloads the configuration **without stopping NGINX**.  
Active connections are **not interrupted**.

```bash
sudo systemctl reload nginx
```

---

### 🔍 Check NGINX Status
Shows if NGINX is running, stopped, or has errors.
```bash
sudo systemctl status nginx
```

---

### 🧪 Test NGINX configuration (IMPORTANT)
Always test config before reload or restart.

```bash
sudo nginx -t
```

If OK:
```bash
sudo systemctl reload nginx
```

---

## 🧪 Syntax Test (`nginx -t`)

Before restarting or reloading NGINX, always test your configuration:

```bash
sudo nginx -t
```

### ✔ What this does:
- Checks if your NGINX config files are written correctly
- Finds missing semicolons, wrong paths, bad directives, etc.

### If everything is okay, you will see:
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

### If there is an error:
NGINX will show the filename and line number so you can fix it.

---

## 🌐 Serving Static HTML Websites

By default, NGINX serves static pages (HTML, CSS, JS, images, etc.) from:

```
/var/www/html
```

If you open a browser and visit:

```
http://your-server-ip/
```

NGINX will load the file:

```
/var/www/html/index.html
```

So static content works **out of the box**.

---

## 📁 Creating a Custom Web Root Folder

You can host your own website by creating your own folder.

### Example: create a custom folder
```bash
sudo mkdir -p /var/www/mysite
```

### Add a sample index file
```bash
echo "<h1>Welcome to MySite</h1>" | sudo tee /var/www/mysite/index.html
```

### Give NGINX permission to read it
```bash
sudo chown -R www-data:www-data /var/www/mysite
sudo chmod -R 755 /var/www/mysite
```

---

## 🏗 Configuring NGINX to Use the Custom Folder

Edit (or create) your site config:

```bash
sudo nano /etc/nginx/sites-available/mysite.conf
```

Add:

```nginx
server {
    listen 80;
    server_name mysite.local;
    root /var/www/mysite;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable the site:

```bash
sudo ln -s /etc/nginx/sites-available/mysite.conf /etc/nginx/sites-enabled/
```

Test & reload:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 📄 Configuring Index Files

The `index` directive tells NGINX which file to load first.

Example:

```nginx
index index.html index.htm home.html;
```

NGINX will look in this order:
1. `index.html`
2. `index.htm`
3. `home.html`

If none exist, you get a **403 Forbidden** or **404 Not Found**.

---

## 🚀 Final Flow (Simple Explanation)

1. You create the website folder  
2. Put an `index.html` file inside  
3. Point a server block to that folder  
4. Test config with `nginx -t`  
5. Reload NGINX  

Your site is live! 🎉

---

