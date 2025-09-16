# Graylog-SOC-Lab

![Graylog Image](/images/graylog.png)

---

## 📌 Overview

A **hands-on SOC lab** using **Graylog Open Source** to **centralize, monitor, and analyze logs from multiple sources**. This lab is designed for cybersecurity practice, stream creation, alerting, and dashboard visualization.

---

## 📖 Setup Guide

### 1️⃣ Docker Installation & Project Setup 🐳

**1.1 Install Docker and Docker Compose:**

```bash
# Install Docker Desktop from official site
https://www.docker.com/products/docker-desktop/

# Verify installation
docker --version
docker compose version
```

**1.2 Clone the GitHub Project:**

```bash
git clone https://github.com/ilyess-sellami/Graylog-SOC-Lab.git
cd Graylog-SOC-Lab
```

This project already contains:
- `docker-compose.yml`
- `.env` template

**1.3 Edit the .env file:**

```bash
nano .env
```

Set the following values:

```bash
# A long random secret (>=64 chars). Generate with: pwgen -N 1 -s 96
GRAYLOG_PASSWORD_SECRET=""

# Root password for web UI (hash of your chosen password).
GRAYLOG_ROOT_PASSWORD_SHA2=""
```

💡 Tip: Generate your SHA256 hash using:

```bash
echo -n "YourPasswordHere" | shasum -a 256
```

## 2️⃣ Start Graylog Using Docker Compose 🐋

```bash
docker compose up -d
```

- Check running containers:

```bash
docker compose ps
docker compose logs -f graylog
```

- You should see a message like:

```bash
Initial configuration is accessible at 0.0.0.0:9000, with username 'admin' and password 'XXXXXX'
```

This is the **temporary preflight password** for the first login.