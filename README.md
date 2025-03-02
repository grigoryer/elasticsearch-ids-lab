# Elastic Search SIEM

## Overview
This project sets up an **Elasticsearch-based Security information and event management (SIEM)** using an Ubuntu-based Elastic Stack. It centralizes security monitoring by collecting and analyzing endpoint data through **Elastic Agents**.

## Features
- **Centralized Security Monitoring** – Collect logs, metrics, and security events from multiple endpoints.
- **Fleet Management** – Deploy and manage agents across systems.
- **Integrations** – Connect with various security tools and data sources for deeper analysis.
- **Web UI (Kibana)** – Visualize security data, create alerts, and monitor threats.

## Lab Setup
The environment consists of:
- **UTM Ubuntu Virtual Machines** – Virtual Machines run locally to simualte a company network.
- **Elasticsearch Server** – Central data store and processing engine.
- **Kibana** – Web-based UI for analysis and configuration.
- **Fleet & Agents** – Distributed security monitoring across endpoints.

## Installation Guide
1. **[Environment Setup](environment_setup.md)** – Configure the virtual machines, network and servers.
2. **[Elasticsearch & Kibana](elasticsearch_installation.md)** – Install and configure the core Elastic services.
3. **[Fleet Agent Setup](fleet_agent_setup.md)** – Deploy and configure Fleet and Elastic Agents.
4. **[Integration and Testing](integration_testing.md)** – Add security integration, and test rule with a Brute Force Attack.

## Usage
- Access Kibana via: `http://[Server-IP]:5601`
- Monitor agent status in the **Fleet** section.
- Configure alerts and dashboards for security insights.

## Future Improvements
- Implement custom detection rules.
- Integrate with external log sources (e.g., Sysmon, Suricata).
- Automate deployment with Ansible playbook.

---
🔹 *This project is a hands-on learning experience in setting up a security monitoring solution with Elasticsearch.*  
