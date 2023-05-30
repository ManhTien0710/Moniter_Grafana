# install Prometheus
```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz
tar -xvf prometheus-2.43.0.linux-amd64.tar.gz
mv prometheus-2.43.0.linux-amd64 /opt/prometheus

useradd --no-create-home --shell /bin/false prometheus
mkdir /etc/prometheus
mkdir /var/lib/prometheus

cp /opt/prometheus/prometheus /usr/local/bin/
cp /opt/prometheus/promtool /usr/local/bin/
cp -r /opt/prometheus/consoles /etc/prometheus
cp -r /opt/prometheus/console_libraries /etc/prometheus

chown -R prometheus:prometheus /etc/prometheus
chown -R prometheus:prometheus /var/lib/prometheus
chown prometheus:prometheus /usr/local/bin/prometheus
chown prometheus:prometheus /usr/local/bin/promtool
```
# stop firewall
```bash
systemctl stop firewalld.service
systemctl disable firewalld.service
```
# setup file comfig
```bash
echo "
global:
  scrape_interval:     5s
  evaluation_interval: 5s
scrape_configs:
  - job_name: linux
    static_configs:
      - targets: ['172.16.10.86:9100']
        labels:
          alias: vesta_cp
" > /etc/prometheus/prometheus.yml

vi /etc/systemd/system/prometheus.service
```
# check status service
```bash
systemctl daemon-reload
systemctl restart prometheus
systemctl status prometheus
systemctl enable prometheus
```
# install grafana
```bash
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-9.4.7-1.x86_64.rpm
yum install grafana-enterprise-9.4.7-1.x86_64.rpm

git clone https://github.com/percona/grafana-dashboards.git
cp -r grafana-dashboards/dashboards /var/lib/grafana
```
# check status service
```bash
systemctl start grafana-server
systemctl status grafana-server
systemctl enable grafana-server
```
# add datasource Prometheus --> Grafana --> Complete