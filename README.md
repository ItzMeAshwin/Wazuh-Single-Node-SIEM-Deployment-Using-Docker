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
|     Filebeat            |
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
|  (Kibana-like UI)       |
+-------------------------+

