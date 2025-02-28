## Fleet Agent Setup

Fleet is used to centrally manage and monitor Elastic Agents across multiple endpoints. This setup involves creating a Fleet Server, adding agents, and applying policies to manage configurations and integrations.

### Understanding Policies and Integrations in Elasticsearch
In Elasticsearch, **policies** define how agents are configured and managed. A policy determines the **data collection settings**, security rules, and integrations assigned to a group of agents. **Integrations** are modules that allow Fleet agents to collect logs, metrics, and security data from various sources, such as cloud services, operating systems, or network devices. Policies ensure that each agent has the appropriate integrations and settings applied, allowing for streamlined data collection and analysis.

---

### Creating the Fleet Server

1. Add a Fleet Server and create a **Fleet Server policy**.
2. Enter the IP of the server with port `8220`.

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.17.2-arm64.deb
sudo dpkg -i elastic-agent-8.17.2-arm64.deb
sudo systemctl enable elastic-agent
sudo systemctl start elastic-agent
```

3. Create a **Fleet policy** to manage agents. This policy allows agents to receive updates and configurations.
   - The policy name can be anything.
   - The Fleet Server IP should be `https://[IP Address]:8220`.

4. Retrieve Fleet connection commands from the **Fleet section** in Kibana.

```bash
sudo elastic-agent enroll \
  --fleet-server-es=https://192.168.1.117:9200 \
  --fleet-server-service-token=[own-service-token] \
  --fleet-server-policy=fleet-server-policy \
  --fleet-server-es-ca-trusted-fingerprint=[own-ca] \
  --fleet-server-port=8220
```

If everything is done successfully, the hostname should appear with the **Healthy** status.

<img width="1000" alt="Screenshot 2025-02-26 at 18 49 21" src="https://github.com/user-attachments/assets/fb96a6ca-b580-42c8-9b3a-55afd5a94633" />

---

### Adding Agents

1. In the **Fleet section** of Kibana, click **Add Agents**.
2. Create an **Agent Policy**. This policy is used to manage all agents and define integrations.

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.17.2-amd64.deb
sudo dpkg -i elastic-agent-8.17.2-amd64.deb
sudo systemctl enable elastic-agent 
sudo systemctl start elastic-agent 
sudo elastic-agent enroll --url=https://192.168.1.117:8220 --enrollment-token=[enrollment-token]
```

After adding the agent, you will see its hostname with the **Healthy** status.

<img width="1000" alt="Screenshot 2025-02-26 at 21 12 31" src="https://github.com/user-attachments/assets/caa5f9a4-e8dc-46c1-9c85-9e982e49ebed" />

---
[Next: Integrations](integrations.md)
