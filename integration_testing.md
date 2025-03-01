## Integration and Testing

Once Fleet agents are installed and enrolled, the next step is to integrate **Elastic Defend** and enable security rules to detect threats on the endpoint. This section covers adding integrations, enabling detection rules, and testing with brute-force attacks.

---

### Adding the Elastic Defend Integration

Navigate to **Fleet** in Kibana and go to the **Agent Policies** tab.
Select the policy associated with the endpoint.
Click **Add Integration** and select **Elastic Defend**.
Provide an **Integration Name**.
In the **Configuration Settings** section:
   - Select **Traditional Endpoints** as the deployment type.
   - Choose **Complete EDR** to enable full detection and response capabilities.
Add the integration to the existing host.

Once added, Elastic Defend will begin monitoring system activities, collecting logs, and generating alerts for suspicious behavior.

---

### Enabling Detection Rules

Navigate to **Security** > **Rules** in Kibana.
You can enable detection rules based on the MITRE ATT&CK framework or relevant keywords. For our SSH brute force demo we need to enable the rule.

Since our endpoint is running on Ubuntu, enable the following Linux-based rules:
   - **Potential Internal Linux SSH Brute Force Detected**
   - **Potential Internal Linux RDP Brute Force Detected**

Additional useful integrations include:
   - **Auditd Logs**: Provides system audit logs for security monitoring.
   - **Network Packet Capture**: Captures raw network traffic for deeper analysis.
   - **Snort**: Uses rule-based intrusion detection to analyze network packets.

These rules and integrations enhance visibility into endpoint security events.

<img width="1069" alt="Screenshot 2025-02-28 at 17 24 04" src="https://github.com/user-attachments/assets/88ef7495-66dc-413d-bc48-e3ea1d388176" />

---

### Testing with Kali Linux

To test the setup, we simulate an SSH brute-force attack using **Hydra**, a common penetration testing tool. Since this is a lab environment, we are only testing locally and will not expose SSH to the public internet.

On the Kali machine, run the following command to brute-force SSH authentication:
   ```bash
   hydra -L user -P /usr/share/wordlists/rockyou.txt -t 4 [IP] ssh
   ```
   - `-L user` specifies the username list.
   - `-P /usr/share/wordlists/rockyou.txt` is the password list.
   - `-t 4` sets four parallel attack threads.
   - `[IP]` is the IP address of the target endpoint.
   - `ssh` specifies the protocol being attacked.

I will be running this command

```bash
   hydra -L moose -P /usr/share/wordlists/rockyou.txt -t 4 192.168.1.131 ssh
   ```

As the attack runs, Elastic Defend will detect brute-force attempts and generate an alert.
<img width="1154" alt="Screenshot 2025-02-28 at 17 20 30" src="https://github.com/user-attachments/assets/14491fdf-404e-402c-bc9e-1b6286a9d563" />

Navigate to **Security** > **Alerts** in Kibana to verify the detection.
The alert should show details such as:
   - **Source IP**: The Kali machine’s IP.
   - **Target IP**: The Ubuntu endpoint.
   - **Rule Triggered**: Linux SSH Bruteforce Detection.

<img width="1035" alt="Screenshot 2025-02-28 at 17 35 14" src="https://github.com/user-attachments/assets/aaf25ae4-5241-44fd-8686-5ea26e2312ee" />

---


### Conclusion

By integrating Elastic Defend and enabling detection rules, we can actively monitor and respond to security threats. The test using Hydra confirms that the setup detects brute-force attempts, validating the effectiveness of the deployed rules and integrations.

---
