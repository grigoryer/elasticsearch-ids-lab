## Elasticsearch Installation
**The entire elasticsearch documentation can be found [here ](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html).**


In order to copy/paste the commands into your terminal you will need to connect through ssh. The servers do not support mouse or spice tools. The way too connect with ssh will be 
```
ssh [user]@[hostname or IP address]
```

Since we are running an ubuntu server, we will be using the deb package. Since all the packages are signed we need the public signing key first. Then install the apt-transport-https, which ensures packages can be installed over https instead of unencrpyted http. 

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
sudo apt-get install apt-transport-https
```

Then add the elastic search repository to the APT sources list using the public singing key. Then update and install elasticsearch.
```
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt-get update && sudo apt-get install elasticsearch
```

After installation the security autoconfiguration txt will be pasted. The super user generated password will be shown here. 

<img width="652" alt="Screenshot 2025-02-27 at 15 56 52" src="https://github.com/user-attachments/assets/bd0e52f1-081a-4ff6-85d8-4aaa21a10bd9" />

Great, elastic search is now installed now we need to enable and configure it. Reload the daemon and enable and start the ubuntu server. 

```
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch        
sudo systemctl start elasticsearch
```

After the service starts, we can configure the IP address in order to access elasticsearch from any device on the network. The location of the configation file is ``/etc/elasticsearch/elasticsearch.yml`` Too modify I will use nano, however any text editor will work.

```
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Find and change the ``#network.host: localhost`` -> ``network.host: [IP Address]``
Confirm that http is connecrable from anywhere with ``http.host: 0.0.0.0``

Now we can check the status of elasticsearch.

```
export ELASTIC_PASSWORD=[Password]
sudo curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:$ELASTIC_PASSWORD https://192.168.1.117:9200 
```

The status is up and viewable if the curl command returns a JSON file.

<img width="885" alt="Screenshot 2025-02-27 at 16 25 18" src="https://github.com/user-attachments/assets/a7937228-9a14-4ada-a73e-b5e1278e2264" />. 


## Kibana Installation

Now we will install the visualation part of the ELK stack, called kibana. This will be the UI and where we will connect to view our logs and add integrartions. Since we have already added the elasticsearch APT Repository we will only need to install. 

```
sudo apt-get update && sudo apt-get install kibana
sudo systemctl daemon-reload
sudo systemctl enable kibana
```

Next in order to encrypt and save the xpack.encrpytion_keys. They all have there own uses however we need the encryptedSavedObjects key which is in charge of encrpyting dashboards, ui configurations, api's, etc...

```
sudo /usr/share/kibana/bin/kibana-encryption-keys generate
```

In addition to adding the encrpytion key, we also need to configure the host of kibana in the configuration file at ``/etc/kibana/kibana.yml``.

Find and replace the following items
* ``#server.host: localhost`` -> ``server.host: [IP Address]``
* ``elasticsearch.hosts: ['localhost:9200']`` -> ``elasticsearch.hosts: ['https://192.168.1.117:9200']``
* ``xpack.encryptedSavedObjects.encryptionKey: "key"``

Now start kibana

```
sudo systemctl start kibana
````

## Kibana Connection

From a browser on any machine on the network you can connect to kibana with the url ``http://192.168.1.117:5601/``

You will now need to generate the enrollment token with the command
```
cd /usr/share/elasticsearch
sudo bin/elasticsearch-create-enrollment-token -s kibana
```

The verfication code will be in the kibana logs which can be found by looking at the status of kibana or through the logs with the commands
```
sudo systemctl status kibana
sudo tail -n 50 /var/log/kibana/kibana.log
```

After pasting both into the alloted text boxes you can log in with the ``elastic`` username and the generetaed password from elasticsearch.
