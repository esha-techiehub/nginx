## 4. Server Blocks (Virtual Hosts)

NGINX allows you to host **multiple websites on a single server** using **server blocks** (similar to Apache Virtual Hosts).  
Each website gets its own configuration file.

---

## 🌍 Hosting Multiple Websites on a Single Server

Each website (domain) gets:
- its own document root  
- its own `server_name`  
- its own config file in `sites-available/`  
- a symlink in `sites-enabled/`  

Example folder structure:

```
/var/www/
├── site1.com/
│   └── index.html
└── site2.com/
    └── index.html
```

---

## 🆕 Creating a New Domain Config

Create a new file:

```bash
sudo nano /etc/nginx/sites-available/yourdomain.com
```

Add the following:

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /var/www/yourdomain;     # Document root
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Then create the web folder:

```bash
sudo mkdir -p /var/www/yourdomain
sudo chown -R www-data:www-data /var/www/yourdomain
sudo chmod -R 755 /var/www/yourdomain
```

---

## 🔗 Enabling & Disabling Sites

### Enable a site
Create a symbolic link to `sites-enabled`:

```bash
sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
```

### Disable a site
Remove the symlink (safe — config stays in sites-available):

```bash
sudo rm /etc/nginx/sites-enabled/yourdomain.com
```

### Test and reload NGINX
```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 🔀 Redirects (301 / 302)

### 1️⃣ Permanent Redirect (301)
Used when a site has permanently moved.

```nginx
server {
    listen 80;
    server_name olddomain.com;

    return 301 https://newdomain.com$request_uri;
}
```

### 2️⃣ Temporary Redirect (302)
Used when a site is temporarily moved.

```nginx
server {
    listen 80;
    server_name test.com;

    return 302 https://maintenance.com;
}
```

---

## ❗ Custom Error Pages (404, 500, etc.)

Create a folder for error pages:

```bash
mkdir -p /var/www/errors
```

Example custom page:

`/var/www/errors/404.html`
```html
<h1>Oops! Page Not Found (404)</h1>
```

### Add to your server block:

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    root /var/www/yourdomain;

    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;

    location = /404.html {
        root /var/www/errors/;
        internal;
    }

    location = /50x.html {
        root /var/www/errors/;
        internal;
    }
}
```

---

## 📘 Quick Summary

| Feature | Purpose |
|--------|---------|
| **server block** | Defines a single website |
| **server_name** | Domain(s) handled by the block |
| **root** | Folder where website files live |
| **Enable site** | symlink to `sites-enabled` |
| **Redirects** | 301 (permanent), 302 (temporary) |
| **Custom errors** | User-friendly 404/500 pages |

---

