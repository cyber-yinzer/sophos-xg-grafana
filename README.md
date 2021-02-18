# sophos-xg-grafana
Monitoring Sophos XG with SNMP &amp; Grafana

THIS PROJECT IS NOT COMPLETE 2/18/20221

### this project will combine Grafana, Telegraf, and Prometheus 

Grafana: https://grafana.com

Telegraf: https://www.influxdata.com/time-series-platform/telegraf/

Prometheus: https://prometheus.io/docs/introduction/overview/

This is built on Debian 10 and will likely work on Ubuntu

#### Getting Started

```
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y gnupg2 curl software-properties-common
sudo apt-get install -y software-properties-common wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get install -y apt-transport-https
sudo apt install prometheus influxdb influxdb-client
sudo apt install prometheus-node-exporter
sudo systemctl start prometheus-node-exporter
sudo apt-get update
sudo apt install update
sudo apt install grafana
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl status grafana-server
sudo systemctl enable grafana-server.service
sudo systemctl status influxdb
sudo apt-get install snmp snmp-mibs-downloader
sudo mkdir /downloads
cd /downloads
sudo wget https://dl.influxdata.com/telegraf/releases/telegraf_1.17.2-1_amd64.deb
chmod +x telegraf_1.17.2-1_amd64.deb 
sudo chmod +x telegraf_1.17.2-1_amd64.deb 
sudo dpkg -i telegraf_1.17.2-1_amd64.deb 
systemctl enable --now telegraf
systemctl status telegraf
```
 You will need to download the Sophos XG MIB for v17 or v18 here: https://docs.sophos.com/nsg/sophos-firewall/MIB/SOPHOS-XG-MIB.zip
 
 Transfer this to your Grafana box
 
```
sudo cp ~/Downloads/SOPHOS-XG-MIB/SOPHOS-XG-MIB18.txt /usr/share/snmp/mibs/
```

Once complete, you'll want to modify telegraf to create the conf file. It is available in this repository.

```
sudo service telegraf restart
sudo telegraf -config /etc/telegraf/telegraf.d/monitor-xg.conf --test
influx
> use telegraf
> exit
> 
```

Now you'll want to open up your firewall to access the web UI from a browser
```
sudo ufw allow 3000
sudo ufw allow 162
```

To continue the UI tweaks, follow along here: https://wiki.kopacko.us/doku.php?id=resources:linux:debian:grafana
