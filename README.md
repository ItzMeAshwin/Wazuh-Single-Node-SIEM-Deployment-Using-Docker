# Wazuh-Single-Node-SIEM-Deployment-Using-Docker
This project demonstrates how to deploy a production-grade SIEM (Security Information &amp; Event Management) solution using Wazuh on a single-node Docker environment.

It includes:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Filebeat
- Windows Agent Integration
- Log Collection
- Alert Generation
- Detection of brute-force and suspicious activities

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

## 1. Clone the Stable Wazuh Deployment (v4.8.0)

```bash
git clone -b v4.8.0 https://github.com/wazuh/wazuh-docker.git
```

## 2. Navigate to the Single-Node Deployment

```bash
cd wazuh-docker/single-node
```

## 3. Deploy Wazuh Using Docker Compose

```bash
docker compose up -d
```

This will automatically pull the images:

- wazuh/wazuh-manager:4.8.0
- wazuh/wazuh-indexer:4.8.0
- wazuh/wazuh-dashboard:4.8.0
- wazuh/filebeat:4.8.0

Deployment time: 3–5 minutes

## 4. Access Wazuh Dashboard

```bash
https://localhost:5601
```

## 




