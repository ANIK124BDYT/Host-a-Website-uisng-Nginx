# 🌐 Host a Website on a VPS Using Nginx + HTTPS (Certbot)

This guide helps you host a static HTML website on an Ubuntu VPS using **Nginx** as the web server and **Certbot (Let's Encrypt)** for free HTTPS. It also shows how to redirect a subdomain like `discord.yourdomain.com` to a custom URL (like a Discord invite).

---

## ✅ Requirements

- Ubuntu VPS (e.g., Ubuntu 20.04 or 22.04)
- Root or sudo access
- Domain name (e.g. `yourdomain.com`) pointing to your VPS IP
- HTML website files

---

## 📦 Install Nginx & Certbot

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx certbot python3-certbot-nginx -y
```

## 🌐 Point Your Domain to VPS
Go to your domain registrar DNS settings, and:
- Add an A Record for `@` pointing to your VPS IP
- Add another `A` Record for `www` pointing to the same IP

Example:
```bash
Type: A
Name: @
Value: <your VPS IP>

Type: A
Name: www
Value: <your VPS IP>
```
⚠️ If Using CloudFlare Disable Proxy

## 📁 Upload Your Website Files
```bash
sudo mkdir -p /var/www/yourdomain.com
```
Add your index.html (or upload full site codes inside the `var/www/yourdomain.com`):

## 🖥️ Configure Nginx
```bash
sudo nano /etc/nginx/sites-available/yourdomain.com
```
Paste this:
```code
server {
    listen 80;
    server_name <domain>;
    root /var/www/<folder>;

    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}


```
⚠️ Replace 'yourdomain.com' with your domain

Enable the config:
```bash
sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
```
Test and reload:
```bash
sudo nginx -t
sudo nginx -s reload
```

## 🔐 Enable SSL (HTTPS) with Certbot
Install Certbot:
```bash
sudo apt install certbot python3-certbot-nginx -y
```
Get your certificate:
```bash
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```
This will:
- Get the SSL certificate
- Configure HTTPS
- Automatically redirect HTTP → HTTPS
Test auto-renewal:
```bash
sudo certbot renew --dry-run
```
---

## 🌐 how to redirect a subdomain like discord.yourdomain.com to a custom URL (like a Discord invite)

**1. DNS Setup**
Add an A record in DNS:
```bash
Type: A
Name: discord
Value: <your VPS IP>
```
**2. Nginx Redirect Config**
```bash
sudo nano /etc/nginx/sites-available/discord.yourdomain.com
```
paste this:
```code
server {
    listen 80;
    server_name discord.yourdomain.com;

    return 301 https://discord.com/invite/YOUR_INVITE_CODE;
}
```
Enable it:
```code
sudo ln -s /etc/nginx/sites-available/discord.yourdomain.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
Install SSL:
```code
sudo certbot --nginx -d discord.yourdomain.com
```
---
## 📂 Directory Structure
```code
/var/www/
└── yourdomain.com/
    └── html/
        ├── index.html
        ├── style.css
        └── script.js
```

## ✅ Final Result
Your website is now hosted at:
```code
https://yourdomain.com
```
And optionally, subdomains like:
```code
https://discord.yourdomain.com → redirects to Discord invite
```

---

## 🛠️ Useful Commands

- **Reload Nginx:** ``sudo systemctl reload nginx``
- **Restart Nginx:** ``sudo systemctl restart nginx``
- **Check Nginx errors:** ``sudo journalctl -xe``

**View logs:**
- Access log: ``sudo tail -f /var/log/nginx/access.log``
- Error log: ``sudo tail -f /var/log/nginx/error.log``

## 🙋 Help & Support

If you run into any issues:

- Double-check your domain's DNS settings (make sure it points to your VPS IP).
- Ensure Nginx is running correctly: ``sudo systemctl status nginx``
- Check your Nginx config for errors: ``sudo nginx -t``
- Review error logs: ``sudo tail -f /var/log/nginx/error.log``
- Check if ports 80 and 443 are open on your VPS firewall.

Still stuck?  
Feel free to open an issue in this repository or reach out to the project maintainer.

---

## 👀 I See...

Thanks for reading! Now your website is live and secured with HTTPS.  
Happy hosting 🚀

## 🙌 Credits

- 🧠 Guide written & maintained by [anik124bd](https://github.com/anik124bd)
- 💻 VPS Setup & Nginx Configuration by **Anik**
- 📄 Inspired by real-world deployment needs
- 📢 Spread the word if this helped you!

---

⭐ Star this repo if you found it useful!
