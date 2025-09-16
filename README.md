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

### 2️⃣ Start Graylog Using Docker Compose 🐋

**2.1 Run docker compose:**

```bash
docker compose up -d
```

**2.2 Check running containers:**

```bash
docker compose ps
```

![Graylog Docker Components](/images/graylog_docker_components.png)

```bash
docker compose logs -f graylog
```

You should see a message like:

```bash
Initial configuration is accessible at 0.0.0.0:9000, with username 'admin' and password 'XXXXXX'
```

![Graylog Docker First Time Runed](/images/graylog_first_time.png)

This is the **temporary preflight password** for the first login.

### 3️⃣ Initial Graylog Setup ⚙️

**3.1 Access preflight UI: `http://localhost:9000`**

**3.2 Log in using temporary credentials from Docker logs (username: `admin`, password: `XXXXXX`)**

**3.3 Configure Data Node Certificates:**

- Generate self-signed CA
- Configure default renewal policy
- Provision certificates
- Click **Configuration Finished**

![Graylog Initial Setup](/images/graylog_initial_setup.png)

![Graylog Configuration Successful](/images/graylog_conf_successful.png)

**3.4 Restart Graylog (optional):**

```bash
docker compose restart graylog
```

**Log in with `.env` Password:**

After completing the preflight setup and provisioning Data Node certificates:

- Open your browser: `http://localhost:9000`
- You will see the **Graylog login page**
- Enter the credentials Username: `admin` and Password: the one you configured in your `.env` file

![Graylog Login Page](/images/graylog_login_page.png)