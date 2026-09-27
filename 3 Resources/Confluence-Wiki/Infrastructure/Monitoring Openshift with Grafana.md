---
ai_hash: 28384688a7075e48
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.83
entities: []
relevance: 0.779
source: https://axonivy.atlassian.net/wiki/spaces/AII/pages/3541125616/Monitoring+Openshift+with+Grafana
space: AII
status: reference
tags:
- confluence
- infra
- space/aii
title: Monitoring Openshift with Grafana
topic: infra
type: source
updated: 2018-07-12
---

# Monitoring Openshift with Grafana

> [!info] Imported from Confluence
> Space **AII** · updated 2018-07-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/AII/pages/3541125616/Monitoring+Openshift+with+Grafana)
> Relevance 0.779 · topic `infra`

![[3541125616-grafana.jpg]]

  
  
  
  
We build the Grafana to monitoring the resource, application, container in Openshift. We setup the host which installed the InfluxDB to keep the  details of the system (CPU, RAM, HDD, etc.) and Grafana as the UI to show up the information from InfluxDB.

#### **Server Grafana side:**  Installing InfluxDB  

> wget <span class="nolink">https://dl.influxdata.com/influxdb/releases/influxdb_1.6.0_amd64.deb</span>  
> sudo dpkg -i influxdb_1.6.0_amd64.deb

  
to avoid having to do this every time you reboot your machine, you can also make it start at boot time:

> sudo systemctl enable influxdb  
> sudo systemctl start influxdb  
> systemctl status influxdb

  
InfuxDB configuration files is  /etc/influxdb/influxdb.conf.

what we need to do is find the HTTP authentication line, uncomment it and change it to true, like so:

> \# Determines whether HTTP authentication is enabled.  
> auth-enabled = true

next we need to set up a username and password in InfluxDB, like so:

> \$ influx  
> \> CREATE USER "influx" WITH PASSWORD 'influx_pass' WITH ALL PRIVILEGES

now it’s time to restart the InfluxDB service:

> sudo systemctl restart influxdb

  

#### Installing Grafana

> wget <span class="nolink">https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_5.1.4_amd64.deb</span>  
> sudo apt-get install -y adduser libfontconfig  
> sudo dpkg -i grafana_5.1.4_amd64.deb

to start Grafana and ensure it starts automatically after reboots:

> sudo /bin/systemctl enable grafana-server  
> sudo /bin/systemctl start grafana-server  
> sudo /bin/systemctl daemon-reload

  

#### Configuring Grafana

To configure Grafana we need to browse to http://\<server_ip\>:3000/login and log in with the username admin and password admin .

Once in, we’ll need to click on the “Create your first data source” link and then enter the details of our InfluxDB server. Once done, clicking “Save & Test” should result in a “Success” message appearing.  
  
  
<span class="confluence-embedded-file-wrapper"><img src="https://i2.wp.com/www.oznetnerd.com/wp-content/uploads/2017/06/Grafana_Influx.png?resize=676%2C597" class="confluence-embedded-image confluence-external-resource" data-image-src="https://i2.wp.com/www.oznetnerd.com/wp-content/uploads/2017/06/Grafana_Influx.png?resize=676%2C597" loading="lazy" /></span>  
  
  
Now that we’ve connected Grafana to our InfluxDB database  
  
<span class="confluence-embedded-file-wrapper"><img src="https://i1.wp.com/www.oznetnerd.com/wp-content/uploads/2017/06/Grafana_Graph.png?resize=676%2C247" class="confluence-embedded-image confluence-external-resource" data-image-src="https://i1.wp.com/www.oznetnerd.com/wp-content/uploads/2017/06/Grafana_Graph.png?resize=676%2C247" loading="lazy" /></span>

  

  

#### **Client side:**

#### Installing Telegraf

> wget <span class="nolink">https://dl.influxdata.com/telegraf/releases/telegraf_1.7.1-1_amd64.deb</span>
>
> sudo dpkg -i telegraf_1.7.1-1_amd64.deb

**  
 **we’ll need to edit Telegraf’s configuration file,   /etc/telegraf/telegraf.conf .

we need to locate the \[\[outputs.influxdb\]\]  section, uncomment the username and password lines and make sure their values match those which we set in InfluxDB:

> username = "influx"  
> password = "influx_pass"

 to restart the Telegraf service:

> sudo systemctl restart telegraf

%% ai-graph-start %%

**Related notes:**
- [[14. Deploy openshift cluster monitoring]]
- [[Axonivycloud - Monitoring EKS cluster using Prometheus and Grafana]]
- [[Monitoring with Managed Prometheus]]
- [[Deploy to Kubernetes and get External IP]]
- [[Axonivycloud - Deploy Nginx Ingress for EKS]]

%% ai-graph-end %%