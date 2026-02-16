# 📬 MailWizz Self-Hosted Infrastructure Project

## 📖 Overview

This project documents my journey of building and deploying a production-style MailWizz email marketing platform using Docker.

Rather than simply deploying a prebuilt image, this project focuses on:

- Infrastructure design
- Environment separation
- Security hardening
- Reverse proxy architecture
- Persistent data strategy
- Backup planning
- Version control discipline
- Documentation-first development

The goal is to treat this as a real SaaS infrastructure build, not just a container deployment.

---

# 🎯 Project Goals

- Build MailWizz using a production-grade Docker architecture
- Separate development and production environments
- Implement proper reverse proxy configuration
- Use secure environment variable management
- Maintain persistent database storage
- Implement backup strategy
- Document the entire process clearly
- Avoid undocumented “mystery fixes”

---

# 🏗 Architecture Overview

## Production Flow

```
Cloudflare
    ↓
Nginx (reverse proxy container)
    ↓
PHP-FPM (MailWizz container)
    ↓
MySQL (database container)
```

## Components

### Nginx
- Reverse proxy
- SSL termination
- Routes traffic to PHP-FPM

### PHP-FPM
- Runs MailWizz application

### MySQL
- Persistent database
- Stored in Docker volume

### Optional Extensions
- Redis (caching)
- Dedicated cron container
- Health checks
- Log aggregation

---

# 🗂 Project Structure

```
mailwizz/
│
├── docker/
│   ├── nginx/
│   │   └── nginx.conf
│   ├── php/
│   │   └── Dockerfile
│   └── mysql/
│
├── docker-compose.yml
├── docker-compose.dev.yml
├── .env
├── .env.example
│
├── web/                # MailWizz source files
│
├── logs/
│   ├── nginx/
│   └── php/
│
└── scripts/
    ├── backup.sh
    └── restore.sh
```

---

# 🌎 Environment Strategy

This project uses two environments:

## Development (Local)

- Used for testing and iteration
- Runs via `docker-compose.dev.yml`
- No public exposure
- Self-signed or no SSL required

## Production (VPS)

- Publicly accessible
- Behind Cloudflare proxy
- SSL enabled (Full Strict mode)
- Hardened Ubuntu server
- UFW firewall enabled
- SSH key authentication only

---

# 🔐 Security Strategy

## VPS Hardening

- Disable root login
- SSH keys only
- UFW firewall enabled
- Automatic security updates
- Only ports 22, 80, 443 open

## Application Security

- Environment variables stored in `.env`
- `.env` never committed
- `.env.example` committed
- No secrets inside docker-compose files

---

# 🐳 Docker Strategy

This project avoids copy-pasted stacks.

Each service is explicitly defined:

- `nginx`
- `php-fpm`
- `mysql`

## Docker Best Practices Used

- Explicit volumes
- `restart: unless-stopped`
- Health checks (optional)
- Isolated service networking
- No hardcoded credentials

---

# 🧠 Lessons This Project Is Designed to Teach

- The difference between “it works” and “it’s engineered”
- How reverse proxies function
- How environment variables protect secrets
- Why volume management matters
- The importance of documentation
- Infrastructure thinking over feature thinking

---

# 🚀 Future Improvements

- Add Redis for caching
- Add monitoring (Prometheus / Grafana)
- Implement CI/CD pipeline
- Add automated backup scheduling
- Add staging environment
- Write deployment automation scripts
- Add healthcheck endpoints

---

# 📌 Why This Project Exists

I previously deployed systems without documenting the process.  
This repository exists to correct that mistake.

Every change will be committed clearly.

Examples:

```
feat: initial infrastructure layout
fix: nginx fastcgi configuration
refactor: separated dev and prod compose files
docs: added architecture documentation
```

The goal is to build discipline, not just software.

---

# 👨‍💻 Author Notes

This project represents a shift from:

> “Deploying containers”

to

> “Engineering infrastructure”

It prioritizes clarity, structure, and production thinking over speed.

This repository represents a documented engineering journey, not just a deployed application.
