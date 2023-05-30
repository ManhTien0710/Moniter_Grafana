# monitoring linux server
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.4.0/node_exporter-1.4.0.linux-amd64.tar.gz
tar -xvf node_exporter-1.4.0.linux-amd64.tar.gz
cd ./node_exporter-1.4.0.linux-amd64/
mv node_exporter /usr/local/bin/

useradd -rs /bin/false node_exporter
chown node_exporter:node_exporter /usr/local/bin/node_exporter
```
# open port
```bash
firewall-cmd --zone=public --add-port=9100/tcp --permanent
firewall-cmd --reload
```
# setup file config
```bash
vi /etc/systemd/system/node_exporter.service
```
# check status service
```bash
systemctl daemon-reload
systemctl start node_exporter
systemctl status node_exporter
systemctl enable node_exporter
```
# Link Dashboard Node Exporter linux
```bash
https://grafana.com/grafana/dashboards/11074-node-exporter-for-prometheus-dashboard-en-v20201010/
ID: 11074
```