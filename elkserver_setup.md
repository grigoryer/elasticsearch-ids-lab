## Elasticsearch Installation
**The entire Elasticsearch documentation can be found [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html).**

To copy and paste commands into your terminal, you will need to connect via SSH. The servers do not support mouse input or SPICE tools. To connect using SSH, use:

```
ssh [user]@[hostname or IP address]
```

Since we are running an Ubuntu server, we will use the **Deb** package. Because all packages are signed, we first need to retrieve the public signing key. Then, install `apt-transport-https`, which ensures packages can be installed over **HTTPS** instead of unencrypted **HTTP**.

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
sudo apt-get install apt-transport-https
```

Next, add the Elasticsearch repository to the **APT sources list** using the public signing key. Then, update and install Elasticsearch:

```
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt-get update && sudo apt-get install elasticsearch
```

After installation, the **security auto-configuration** text will be displayed. The **superuser password** will be generated and shown here.

<img width="652" alt="Screenshot 2025-02-27 at 15 56 52" src="https://github.com/user-attachments/assets/bd0e52f1-081a-4ff6-85d8-4aaa21a10bd9" />

Now that Elasticsearch is installed, we need to enable and configure it. Reload the daemon, then enable and start the service:

```
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch        
sudo systemctl start elasticsearch
```

After starting the service, we can configure the IP address to allow access to Elasticsearch from any device on the network. The configuration file is located at `/etc/elasticsearch/elasticsearch.yml`. To modify it, use `nano` or any text editor of your choice:

```
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Find and modify the following lines:
- `#network.host: localhost` → `network.host: [IP Address]`
- Confirm that HTTP is connectable from anywhere by setting: `http.host: 0.0.0.0`

Now, check the status of Elasticsearch:

```
export ELASTIC_PASSWORD=[Password]
sudo curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD https://192.168.1.117:9200 
```

If the command returns a **JSON file**, then Elasticsearch is successfully running.

<img width="885" alt="Screenshot 2025-02-27 at 16 25 18" src="https://github.com/user-attachments/assets/a7937228-9a14-4ada-a73e-b5e1278e2264" />. 

---

## Kibana Installation

Now, we will install the visualization tool of the **ELK stack**, called **Kibana**. This UI will allow us to view logs and add integrations. Since we have already added the **Elasticsearch APT Repository**, we only need to install Kibana:

```
sudo apt-get update && sudo apt-get install kibana
sudo systemctl daemon-reload
sudo systemctl enable kibana
```

Next, to encrypt and save the **xpack.encryption_keys**, run the following command. These keys serve different purposes, but we specifically need the **encryptedSavedObjects** key, which encrypts dashboards, UI configurations, APIs, and more:

```
sudo /usr/share/kibana/bin/kibana-encryption-keys generate
```

In addition to adding the encryption key, we also need to configure Kibana's host in the configuration file located at `/etc/kibana/kibana.yml`.

Modify the following settings:
- `#server.host: localhost` → `server.host: [IP Address]`
- `elasticsearch.hosts: ['http://localhost:9200']` → `elasticsearch.hosts: ['https://192.168.1.117:9200']`
- `xpack.encryptedSavedObjects.encryptionKey: "[Key]"`

Now, start Kibana:

```
sudo systemctl start kibana
```

---

## Kibana Connection

From a browser on any machine on the network, you can connect to Kibana using:

```
http://192.168.1.117:5601/
```

You will now need to generate the **enrollment token** with the following command:

```
cd /usr/share/elasticsearch
sudo bin/elasticsearch-create-enrollment-token -s kibana
```

The **verification code** will be in the Kibana logs. You can find it by checking the service status or reading the logs:

```
sudo systemctl status kibana
sudo tail -n 50 /var/log/kibana/kibana.log
```

After pasting both the **enrollment token** and **verification code** into their respective text boxes, you can log in using the `elastic` username and the **generated password** from Elasticsearch.

---

[Next: Fleet and Agent Setup](fleet_agent_setup.md)
