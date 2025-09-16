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