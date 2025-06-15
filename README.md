# AltSchool Cloud Engineering Exam Project: Professional Web Server Deployment

**Name:** Oyindamola Ibrahim  
**AltSchool ID:** ALT/SOE/024/5808  
**Program:** Cloud Engineering (Tinyuka 2024)

---

## Project Title: Mentli – The Future of Peer-to-Peer Business Coaching

This project demonstrates a complete cloud-native deployment using AWS, Nginx, and domain integration with SSL. It includes setting up an EC2 instance, an Nginx reverse proxy, a Node.js server, DNS routing with Cloudflare, and SSL (Let's Encrypt) setup.

Mentli is a platform idea that enables peer-to-peer business coaching. This project reflects what a real-world cloud deployment for a startup would look like from scratch.

---

## 🌐 Live Deployment

- **Custom Domain:** [https://yindaibrahim.xyz](https://yindaibrahim.xyz)
- **Public IP (for testing):** [http://16.171.24.246](http://16.171.24.246)

![Mentli Screenshot](public/landing-page.png)

---

## ⚙️ Technical Implementation

### 1. Infrastructure & Server Setup

- EC2 (Ubuntu 22.04) provisioned in `eu-north-1` region
- Security Groups configured:
  - **Launch-Wizard SG:** allows SSH, HTTP, HTTPS
  - **ALB SG:** allows HTTP (80) from 0.0.0.0/0
- Application Load Balancer created for better traffic routing
- Subnets across multiple AZs in same VPC

### 2. App & Web Server Configuration

- Node.js + Express app on port `3000`
- Nginx reverse proxy on port `80` and `443`
- Landing page served via `/public` directory
- PM2 used to keep the Node.js server running

### 3. Domain & DNS Configuration

- Domain purchased from Dynadot: `yindaibrahim.xyz`
- Nameservers updated to Cloudflare:
  - `lady.ns.cloudflare.com`
  - `rory.ns.cloudflare.com`
- DNS records:
  - `A` record for root domain pointing to ALB IPs
  - `CNAME` for `www` pointing to root

### 4. SSL/HTTPS Configuration

- Let's Encrypt (Certbot) installed on EC2 instance
- SSL certificate successfully issued for domain
- Nginx updated to include:
  ```nginx
  listen 443 ssl;
  ssl_certificate /etc/letsencrypt/live/yindaibrahim.xyz/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/yindaibrahim.xyz/privkey.pem;
  include /etc/letsencrypt/options-ssl-nginx.conf;
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
  ```
- HTTP → HTTPS redirection added for both `www` and root domain

---

## 🧪 Troubleshooting & Lessons Learned

| Issue | Solution |
|-------|----------|
| Dynadot redirection page showed | Nameservers updated to Cloudflare |
| SSL errors (HTTPS not secure) | Installed Certbot and reconfigured Nginx |
| Cloudflare 522 errors | Opened port 443 in security group and verified Nginx |
| Too many redirects | Caused by SSL mode mismatch (Fixed by aligning Full/Strict with Nginx setup) |
| Certbot errors | Fixed broken Nginx syntax, reloaded and tested configuration |
| ALB not routing traffic | Target group reattached to EC2 instance and passed health checks |
| Deployment breaking with repo rename | Preserved remote link via `git remote set-url` and pushed cleanly |

---

## 💡 Project Concept: Mentli

**Overview:**  
A structured peer-to-peer business coaching platform. Entrepreneurs mentor each other in a marketplace environment.

**Value:**  
- Monetizes experience  
- Promotes community-driven growth  
- Supports scale and flexibility with cloud-native infrastructure

---

## 🛠️ What's Left / Future Enhancements

- [ ] Add GitHub Actions for CI/CD
- [ ] Use Docker to containerize the Node app
- [ ] Enable centralized logging via CloudWatch
- [ ] Add firewall hardening (fail2ban, etc.)
- [ ] Automate SSL renewal via cron
- [ ] Clean up DNS redundancy in Cloudflare

---

## 📁 Directory Structure

```
mentli/
├── index.js                # Express server
├── public/
│   ├── index.html          # Landing page
│   ├── style.css           # Tailwind styles
│   └── landing-page.png    # Updated screenshot
├── pm2.config.js
├── package.json
└── README.md
```

---

## 🔗 Submission Details

- **GitHub Repo:** [https://github.com/yindaibrahim/altschool-project-mentli](https://github.com/yindaibrahim/altschool-project-mentli)
- **Deployed App:** [https://yindaibrahim.xyz](https://yindaibrahim.xyz)

---

**Oyindamola Ibrahim**  
Cloud & DevOps Engineer  
