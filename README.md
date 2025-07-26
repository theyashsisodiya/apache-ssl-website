# apache-ssl-website

Absolutely, Yashraj! Below is your full setup **converted to GitHub-friendly Markdown** format. This is perfect for a `README.md` or project documentation.

---

````md
# 🌐 Apache + SSL (Let's Encrypt) Setup on AWS EC2

This guide walks through setting up a secure HTTPS website using Apache2 and Certbot SSL on an AWS EC2 Ubuntu instance for the domain `aws.yash.gt.tc`.

---

## ✅ System Info

| Item         | Details             |
|--------------|----------------------|
| OS           | Ubuntu (AWS EC2)     |
| Web Server   | Apache2              |
| Domain       | aws.yash.gt.tc       |
| HTTP Port    | 80                   |
| HTTPS Port   | 443                  |
| SSL Provider | Let's Encrypt via Certbot |

---

## 1️⃣ Install Apache2

```bash
sudo apt update
sudo apt install apache2
````

**Why?**
Apache is the HTTP server that will serve your website.

---

## 2️⃣ Configure UFW Firewall

```bash
sudo ufw allow 'Apache Full'
sudo ufw allow OpenSSH
```

**Why?**

* `Apache Full` opens both ports 80 (HTTP) and 443 (HTTPS).
* `OpenSSH` ensures you don’t lose SSH access (port 22).

> If UFW isn’t active or you're using AWS Security Groups, make sure the necessary ports are open in the AWS console.

---

## 3️⃣ Create Document Root & HTML Page

```bash
sudo mkdir -p /var/www/aws.yash.gt.tc
echo "<h1>Welcome to aws.yash.gt.tc</h1>" | sudo tee /var/www/aws.yash.gt.tc/index.html
```

**Why?**
Creates a basic HTML file to serve via Apache.

---

## 4️⃣ Create Apache Virtual Host (Port 80)

```bash
sudo nano /etc/apache2/sites-available/aws.yash.gt.tc-80.conf
```

**Paste this configuration:**

```apache
<VirtualHost *:80>
    ServerName aws.yash.gt.tc
    DocumentRoot /var/www/aws.yash.gt.tc

    <Directory /var/www/aws.yash.gt.tc>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/aws_error.log
    CustomLog ${APACHE_LOG_DIR}/aws_access.log combined
</VirtualHost>
```

### Enable Site + Reload Apache

```bash
sudo a2ensite aws.yash.gt.tc-80.conf
sudo systemctl reload apache2
```

---

## 5️⃣ Troubleshooting Error: curl (7)

### ❌ Error:

```bash
curl: (7) Failed to connect to aws.yash.gt.tc port 80: Connection refused
```

### ✅ Fix:

* Check Apache:

  ```bash
  sudo systemctl status apache2
  ```
* Check DNS:

  ```bash
  nslookup aws.yash.gt.tc
  ```

---

## 6️⃣ Verify Apache is Working

* Open `http://aws.yash.gt.tc` in your browser.
* You should see the welcome message.

---

## 7️⃣ Install Certbot

```bash
sudo apt install certbot python3-certbot-apache
```

**Why?**
Certbot automates the process of requesting and configuring an SSL certificate.

---

## 8️⃣ Get SSL Certificate

```bash
sudo certbot --apache -d aws.yash.gt.tc
```

**What happens?**

* Certbot verifies domain via port 80.
* Fetches SSL cert from Let’s Encrypt.
* Auto-creates:
  `/etc/apache2/sites-available/aws.yash.gt.tc-le-ssl.conf`

✅ Output:

```
Certificate is saved at: /etc/letsencrypt/live/aws.yash.gt.tc/fullchain.pem
Private key is saved at: /etc/letsencrypt/live/aws.yash.gt.tc/privkey.pem
```

---

## 9️⃣ Test HTTPS

### ✅ Browser:

Visit:
`https://aws.yash.gt.tc` → You should see the secure padlock.

### ✅ Terminal:

```bash
curl -I https://aws.yash.gt.tc
```

Expected output:

```
HTTP/2 200 OK
```

---

## 🔁 Optional: Test Auto-Renewal

```bash
sudo certbot renew --dry-run
```

**Why?**
Ensures your SSL certificate will auto-renew without issues.

---

## ✅ Final Checklist

| Task                        | Status |
| --------------------------- | ------ |
| Apache Installed            | ✅      |
| Firewall Configured         | ✅      |
| Virtual Host Created (HTTP) | ✅      |
| DNS → EC2 IP Verified       | ✅      |
| Certbot Installed           | ✅      |
| SSL Certificate Installed   | ✅      |
| HTTPS Working               | ✅      |
| Auto-Renewal Enabled        | ✅      |
| index.html Present          | ✅      |

---

## 🔚 Conclusion

You’ve successfully:

* Set up an Apache site
* Secured it with Let's Encrypt SSL
* Verified HTTPS is functional
* Enabled auto-renewal

---

## 📥 Export This Doc

Let me know if you want:

* 🗎 Markdown file (`README.md`)
* 📄 PDF version
* 🐳 Docker version of this setup
* 🌍 Redirect from HTTP → HTTPS

```

Would you like me to generate and send you the Markdown (`README.md`) file or a downloadable PDF version of this guide?
```
