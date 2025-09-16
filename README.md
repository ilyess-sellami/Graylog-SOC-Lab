# Graylog-SOC-Lab

![Graylog Image](/images/graylog.png)

---

## 📌 Overview

The **Graylog-SOC-Lab** is a **hands-on Security Operations Center (SOC) lab** designed to **centralize, monitor, and analyze logs from multiple sources** using Graylog Open Source. This lab allows cybersecurity practitioners and learners to practice **log collection, stream creation, alerting, and dashboard visualization** in a controlled environment.

---

## 🧩 Components

- **🐳 Docker and Docker Compose**: containerize Graylog, MongoDB, and Elasticsearch for easy deployment and portability.

- **⚡ Graylog Server**: core log management platform handling ingestion, storage, and dashboards.

- **🔑 Graylog Sidecar Agent**: lightweight log collector installed on servers to manage Beats collectors.

- **📦 Filebeat / Auditbeat**: agents that monitor system, application, and security logs.

- **🖥️ Ubuntu Server**: acts as a log source sending system, Apache, and custom application logs.

- **🌐 Web UI / Dashboard**: visualize logs, configure streams, and manage alerts.

- **🔐 API Tokens**: ensure secure communication between Sidecar agents and the Graylog server.

---

## 🏗️ Architecture

- 📌 Logs are collected by **Filebeat or Auditbeat** on the Ubuntu Server.
- 📌 Graylog Sidecar manages these collectors and forwards logs to the Graylog Server via the **Beats input**.
- 📌 Graylog Server **stores logs**.
- 📌 The server provides **searching, alerting, and dashboard visualization** for logs.
- 📌 Security analysts **access logs and dashboards** through the Web UI at http://localhost:9000.

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

**3.5 Log in with `.env` Password:**

After completing the preflight setup and provisioning Data Node certificates:

- Open your browser: `http://localhost:9000`
- You will see the **Graylog login page**
- Enter the credentials Username: `admin` and Password: the one you configured in your `.env` file

![Graylog Login Page](/images/graylog_login_page.png)

### 4️⃣ Configure Graylog Inputs ⚡

**4.1 Go to System → Inputs → Select Beats → Launch new input.**

![Graylog Input Beats](/images/graylog_input_beats.png)

**4.2 Configure example settings:**

- **Title:** `Ubuntu Server`
- **Port:** `5044`
- **Bind address:** `0.0.0.0`

![Graylog Input Configuration](/images/graylog_input_conf.png)

**4.4 Click Save → Input should show Running**

### 5️⃣ Generate API Token for Sidecar 🔑

**5.1 Go to System → Sidecars in Graylog.**

**5.2 Click Actions → `Create or reuse a token for the graylog-sidecar`.**

![Graylog Sidecars Overview](/images/graylog_sidecars_overview.png)

**5.3 Give it a name (e.g., `Ubuntu Server Token`).**

![Graylog Sidecar Token](/images/graylog_sidecar_token.png)

**5.4 Copy the generated token – you will use this in the Sidecar configuration on your Ubuntu Server.**

```yaml
server_api_token: "<YOUR_GENERATED_API_TOKEN>"
```

### 6️⃣ Install Graylog Sidecar Agent on Ubuntu Server

Graylog Sidecar acts as a lightweight collector that forwards logs to your Graylog server.

**6.1 Add Graylog Sidecar repository and install:**

```bash
wget https://packages.graylog2.org/repo/packages/graylog-sidecar-repository_1-5_all.deb
sudo dpkg -i graylog-sidecar-repository_1-5_all.deb
sudo apt-get update
sudo apt-get install graylog-sidecar
```

**6.2 Configure the Sidecar:**

```bash
sudo nano /etc/graylog/sidecar/sidecar.yml
```

Update at minimum:

```bash
server_url: "http://<GRAYLOG_SERVER_IP>:9000/api/"
server_api_token: "<YOUR_GRAYLOG_API_TOKEN>"
node_name: "ubuntu-server-01"
```

**6.3 Install and start Sidecar as a system service:**

```bash
sudo graylog-sidecar -service install
sudo systemctl enable graylog-sidecar
sudo systemctl start graylog-sidecar
sudo systemctl status graylog-sidecar
```

### 7️⃣ Verify Logs in Graylog Dashboard 🔍

**7.1 Go to System → Sidecars.**

- You should see the Sidecar listed with the name `ubuntu-server-01`.
- Ensure the status shows **Running** and it is connected to the Graylog server.

![Graylog Sidecar Ubuntu Server](/images/graylog_sidecar_ubuntu_server.png)

**7.2 Go to Search in Graylog.**

You should see incoming logs from your Ubuntu server, such as Apache access and error logs.
