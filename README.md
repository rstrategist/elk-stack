# ELK Stack Overview

A brief summary of how to set up the ELK stack on a Debian machine.

ELK Stack or Elastic Stack is a powerful logging and analytics solution used for searching, analysing, and visualising log data in real-time.

It is comprised of **E**lasticsearch, **L**ogstash, **K**ibana and Beats.

If you need to go into more details, feel free to check out the following two links. This repo is just a summary of these plus some additional notes I have made:

[Installing Elastic Stack](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)

[Installing Elastic Stack on Debian](https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html)

## Architecture Components

1. **Elasticsearch** – A distributed search and analytics engine that stores and indexes log data for fast querying.  
2. **Logstash** – A data processing pipeline that ingests, transforms, and ships logs to Elasticsearch.  
3. **Kibana** – A visualization and analytics tool for exploring and monitoring log data stored in Elasticsearch.  
4. **Filebeat** – A lightweight log shipper that forwards log files to Logstash or Elasticsearch efficiently.

## Architecture Diagram

![image](https://github.com/user-attachments/assets/90d8247f-4b73-4d07-8539-5847b1cd88ec)
Image from: [elastic.co](https://www.elastic.co/elastic-stack)

## Installation and Enablement Steps

It is criticial that each component is installed in the correct order and with the same version number.

1. Download and install Elasticsearch:
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```
Make sure the apt-transport-https package is installed:
```
sudo apt-get install apt-transport-https
```
Save the repository definition to /etc/apt/sources.list.d/elastic-8.x.list:
```
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```
Update package lists to latest in the repositories and install the Elasticsearch Debian package:
```
sudo apt-get update && sudo apt-get install elasticsearch
```
Elasticsearch security features are enabled by default, generating a superuser password upon installation:
```
The generated password for the elastic built-in superuser is : ###############
```
2. Install Kibana:
```
sudo apt-get install kibana
```
3. Install Logstash:
```
sudo apt-get install logstash
```
4. Install Filebeat:
```
sudo apt-get install filebeat
```
## Enable and start all services:
```
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
sudo systemctl enable filebeat
sudo systemctl enable logstash.service
sudo systemctl enable kibana.service
```
Start services:
```
sudo systemctl start elasticsearch.service
sudo systemctl start filebeat
sudo systemctl start logstash.service
sudo systemctl start kibana.service
```
Check if the services are running:
```
ps aux | grep elastic 
ps aux | grep kibana
```
## Additional Configuration for Nginx:
Check if **Nginx** is installed and install the full version if needed:
```
sudo apt-cache search nginx 
sudo apt-get install nginx-full 
```
Check log files:
```
ls /var/log/nginx/
cat /var/log/nginx/access.log 
cat /var/log/nginx/error.log 
```
### Troubleshooting Elasticsearch
If Elasticsearch is not starting, reload the **systemd** daemon and enable the service to start automatically:
```
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
```
To manually start Elasticsearch:
```
sudo systemctl start elasticsearch.service
```

