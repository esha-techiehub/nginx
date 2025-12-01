# Complete NGINX Syllabus (Beginner → Advanced)

Perfect for DevOps, Cloud, Web Hosting, and Interviews

## 1. Introduction to NGINX
- What is NGINX?
- Why NGINX is faster than Apache (event-driven model)
- Use cases:
  - Web server
  - Reverse proxy
  - Load balancer
  - API gateway
  - Static content hosting
- Installing NGINX on:
  - Linux (Ubuntu/CentOS)
  - Windows (optional)

## 2. NGINX Folder Structure & Files
- /etc/nginx/nginx.conf – Main config
- /etc/nginx/sites-available/
- /etc/nginx/sites-enabled/
- /var/www/html/
- /etc/nginx/conf.d/*.conf
- Log files:
  - access.log
  - error.log

## 3. Basic NGINX Configuration
- Understanding events, http, server, location blocks
- Starting, stopping, restarting NGINX
  - systemctl start nginx
  - systemctl reload nginx
- Syntax test
  - nginx -t
- Serving static HTML websites
- Creating a custom web root folder
- Configuring index files

## 4. Server Blocks (Virtual Hosts)
- Hosting multiple websites on a single server
- Creating a new domain config:
  - server_name yourdomain.com;
  - root /var/www/yourdomain;
- Enabling & disabling sites
- Redirects: 301, 302
- Custom error pages (404, 500)

## 5. Location Blocks (Very Important)
- Types:
  - location /
  - location /images/
  - location =
  - location ~ (regex)
  - location ^~
- Priority order for matching
- Serving static files
- Securing directories

## 6. Reverse Proxy Concepts
- What is a reverse proxy?
- Upstream servers
- Basic reverse proxy setup:
  - proxy_pass http://127.0.0.1:3000;
- Passing headers
- Handling timeouts
- Proxy cache basics

## 7. Load Balancing
- Round robin
- Least connections
- IP hash
- Load balancing config using upstream block
- Health checks

## 8. SSL/TLS in NGINX
- Self-signed certificates
- Installing free Let's Encrypt SSL
- Auto-renewal with cron
- Enforcing HTTPS redirect
- HSTS headers

## 9. Caching in NGINX
- Static file caching
- proxy_cache configuration
- Cache zones
- Cache bypass rules
- Cache purging (optional)

## 10. Security Hardening
- Restricting IP addresses
- Rate limiting
- Preventing DDoS basics
- Adding security headers:
  - HSTS
  - X-Frame-Options
  - X-Content-Type-Options
- Disabling server tokens

## 11. Performance Tuning
- worker_processes
- Keepalive tuning
- Buffering
- Gzip compression
- HTTP/2 enablement
- Logging control

## 12. NGINX for Applications
- Serving Node.js apps
- Serving Python Flask/Django apps
- Serving Java Spring Boot apps
- Docker + NGINX reverse proxy
- NGINX + CI/CD basics

## 13. NGINX in Cloud
- In AWS (EC2 + ALB)
- In Azure
- In GCP
- NGINX ingress controller for Kubernetes (optional DevOps topic)

## 14. Monitoring & Troubleshooting
- Checking NGINX health
- Debug logs
- Access logs analysis
- Common errors:
  - 403 Forbidden
  - 404 Not found
  - 502 Bad Gateway
  - 504 Gateway Timeout
- Restart and reload issues
