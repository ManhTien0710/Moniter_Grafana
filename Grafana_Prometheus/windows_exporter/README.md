# install 
# downloads 
```bash
https://github.com/prometheus-community/windows_exporter/releases
--> windows_exporter-0.20.0-amd64.exe

install on windows
```
# add node into prometheus
```bash
echo "
global:
  scrape_interval:     5s
  evaluation_interval: 5s
scrape_configs:
  - job_name: windows_server
    static_configs:
      - targets: ['172.16.10.86:9180']
        labels:
          alias: vesta_cp
" >> /etc/prometheus/prometheus.yml
```
# add dashboard windows
```bash
https://grafana.com/grafana/dashboards/14694-windows-exporter-dashboard/
```