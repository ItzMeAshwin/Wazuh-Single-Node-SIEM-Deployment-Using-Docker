# Wazuh-Single-Node-SIEM-Deployment-Using-Docker
This project demonstrates how to deploy a production-grade SIEM (Security Information &amp; Event Management) solution using Wazuh on a single-node Docker environment.

It includes:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Windows Agent Integration
- File Integrity Monitoring (FIM)
- Brute-force Detection
- Suspicious Activity Detection

## 📘 Table of Contents

- Architecture
- Prerequisites
- Deployment Steps
- Adding a Windows Agent
- Testing Log Collection & Alerts
- Screenshots
- Project Structure
- Key Features
- Future Enhancements
- Troubleshooting
- Conclusion

## ARCHITECTURE

This project is part of my cybersecurity hands-on portfolio.
```text
+-------------------------+
|     Windows Agent       |
|  (Log Collection, FIM)  |
+------------+------------+
             |
             v
+-------------------------+
|     Wazuh Manager       |
|   (Rules, Decoders)     |
+------------+------------+
             |
             v
+-------------------------+
|        Filebeat         |
| (Forward logs to index) |
+------------+------------+
             |
             v
+-------------------------+
|     Wazuh Indexer       |
| (Elasticsearch backend) |
+------------+------------+
             |
             v
+-------------------------+
|    Wazuh Dashboard      |
|     (Web Interface)     |
+-------------------------+
```

## Prerequisites

Ensure the following are installed:

| Component          | Version   |
|-------------------|-----------|
| Docker Desktop    | Latest    |
| Git               | Latest    |
| Windows 10/11 or WSL | Supported |

> **Note:** Docker Compose is included with Docker Desktop.

## DEPLOYMENT STEPS

### 1. Clone the Stable Wazuh Deployment (v4.8.0)

```bash
git clone -b v4.8.0 https://github.com/wazuh/wazuh-docker.git
```

### 2. Navigate to the Single-Node Deployment

```bash
cd wazuh-docker/single-node
```

### 3. Deploy Wazuh Using Docker Compose

```bash
docker compose up -d
```

This will automatically pull the images:

- wazuh/wazuh-manager:4.8.0
- wazuh/wazuh-indexer:4.8.0
- wazuh/wazuh-dashboard:4.8.0
- wazuh/filebeat:4.8.0

Deployment time: 3–5 minutes

### 4. Access Wazuh Dashboard

```bash
https://localhost:5601
```
```bash
Username: admin
Password: admin
```

## Adding a Windows Agent

#### Step 1 — Download Windows Agent

1. Open **Wazuh Dashboard**
2. Go to **Agents → Deploy new agent**
3. Select:
   - OS: **Windows**
   - Protocol: **TCP (default)**
4. Click **Download MSI Installer**

#### Step 2 — Install Agent

1. Run the MSI installer  
2. Enter:
   - **Manager IP:** your machine's IP  
   - **Enrollment password:** copy from dashboard  
3. Finish installation

#### Step 3 — Approve the Agent

1. Open Wazuh Dashboard  
2. Go to **Agents → Pending**  
3. Click **Approve**

## Test Log Collection & Alerts

You should test and capture screenshots for your GitHub.

🔸 Test 1 — Brute Force Simulation (Windows)

Run in PowerShell:
```bash 
for ($i=1; $i -le 10; $i++) { net use \\localhost\IPC$ /user:test wrongpassword }
```
Expected Wazuh Alerts:

- Authentication failure
- Suspicious brute-force activity
- MITRE ATT&CK mapping (T1110)

🔸 Test 2 — File Integrity Monitoring (FIM)

Create a test file:
```bash
echo "test" > C:\important.txt
```
Modify file:
```bash
echo "changed" >> C:\important.txt
```
Expected Alert:
- File modified (FIM rule)

🔸 Test 3 — Suspicious Process Execution
  ```bash
powershell.exe -ExecutionPolicy Bypass
```
Expected Alert:
- Script execution bypass
- Potential malicious activity

## Screenshots to include in Repository

All screenshots related to the deployment, agent integration, and detections are stored in the screenshots/ folder.

You can preview them below:

#### Wazuh Dashboard
```bash
screenshot_s/dashboard.png
```
#### Connected Windows Agent
```bash 
screenshot_s/agent_connected.png
```
#### Brute Force Detection Alert
```bash
screenshot_s/brute_force_alert.png
```
#### File Integrity Monitoring Alerts
```bash 
screenshot_s/FIM -SS of FIM events
screenshot_s/FIM - Threat hunting Dashboard
```
#### Event Logs in Threat Hunting
```bash
screenshots/threat_hunting_logs.png
```
## Project File Structure
```text
Wazuh-SIEM-Project/
│
├── README.md
├── setup/
│   └── docker-compose.yml
│
├── screenshots/
│   ├── dashboard.png
│   ├── agents.png
│   ├── brute_force_alert.png
│   ├── fim_alert.png
│
└── logs/
```

## Key Features Demonstrated in This Project

- Centralized log management
- Real-time threat detection
- MITRE ATT&CK mapping
- Windows endpoint monitoring
 - File integrity monitoring
- Brute-force detection
- Fully containerized SIEM deployment
- Cloud-ready architecture

## Future Enhancements

These help show growth:

- Deploy multi-node Wazuh cluster
- Integrate Suricata IDS
- Integrate Zeek traffic analyzer
- Add OSQuery agent
- Build dashboards for threat hunting
- Automated alert response (SOAR-like workflow)

## 🚨 Troubleshooting & Common Issues Encountered During Setup

During the setup of Wazuh Single-Node using Docker Desktop on Windows (WSL2 backend), several issues can occur. These are the exact problems we faced, why they happened, and how they were resolved. Adding this section to your project shows deep understanding and real-world troubleshooting skills.
### 1. Docker / WSL2 not opening

Wazuh used too much CPU and froze WSL2.
Fix: Restart Windows or force-stop WSL2 using:
```bash
Stop-Service LxssManager -Force
```

After reboot, Docker works normally again.

### 2. Dashboard not loading (ERR_CONNECTION_REFUSED)

Solution: Use HTTPS, not HTTP.
```bash
https://localhost/
```
Accept the security warning.

### 3. SSL certificate errors (Index or Dashboard failing)

Cause: Missing/corrupted certs in wazuh_indexer_ssl_certs.
Fix: Regenerate:
```bash
docker compose -f generate-indexer-certs.yml run --rm generator
```
### 4. High CPU usage / laptop freezing

Wazuh is heavy and can consume all cores.
Fix:
```bash
Limit WSL2 resources using .wslconfig
```
Inside the .wslconfig
```bash
[wsl2]
processors=2
memory=4GB
Limit container CPU in docker-compose.yml:
cpus: 1.5
```
### 5. Wrong container names for logs

Use the full name shown in docker ps, e.g.:
```bash
docker logs single-node-wazuh.dashboard-1
```

## Conclusion

This project demonstrates a hands-on deployment of a complete SIEM solution using Wazuh. It showcases capabilities in:

- Security Monitoring
- Log Analysis
- Endpoint Detection
- Threat Hunting
- Docker & DevOps
- Incident Simulation
