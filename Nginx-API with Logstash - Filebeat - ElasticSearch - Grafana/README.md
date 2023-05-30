# setup log nginx
```bash
log_format  json_logs  '{"@timestamp":"$time_iso8601","host":"$hostname",'
                            '"server_ip":"$server_addr","client_ip":"$remote_addr",'
                            '"xff":"$http_x_forwarded_for","domain":"$host",'
                            '"url":"$uri","referer":"$http_referer",'
                            '"args":"$args","upstreamtime":"$upstream_response_time",'
                            '"responsetime":"$request_time","request_method":"$request_method",'
                            '"status":"$status","size":"$body_bytes_sent",'
                            '"request_body":"$request_body","request_length":"$request_length",'
                            '"protocol":"$server_protocol","upstreamhost":"$upstream_addr",'
                            '"file_dir":"$request_filename","http_user_agent":"$http_user_agent"'
                            '}';
access_log  /var/log/nginx/access_json.log json_logs;
```

# Filebeat
```bash
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
sudo vi /etc/yum.repos.d/filebeat.repo

yum install filebeat
vi /etc/filebeat/filebeat.yml

systemctl restart filebeat
systemctl status filebeat
systemctl enable filebeat
```
# Logstash
```bash
wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u73-b02/jdk-8u73-linux-x64.rpm"

sudo yum -y localinstall jdk-8u73-linux-x64.rpm
vi /etc/yum.repos.d/logstash.repo

yum -y install logstash

vi /etc/logstash/conf.d/02-beats-input.conf
vi /etc/logstash/conf.d/10-syslog-filter.conf
vi /etc/logstash/conf.d/30-elasticsearch-output.conf

systemctl restart logstash.service
systemctl enable logstash.service
systemctl status logstash.service

sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
```
# ElasticSearch
```bash
yum install wget apt-transport-https default-jre -y
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

sudo vi /etc/yum.repos.d/elasticsearch.repo
sudo yum install elasticsearch

sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch
sudo systemctl status elasticsearch

echo "
network.host: 0.0.0.0
http.port: 9200
discovery.type: single-node
" >> /etc/elasticsearch/elasticsearch.yml

sudo systemctl restart elasticsearch
curl http://127.0.0.1:9200/_cat/indices/logstash-*?v
```

# add Dashboard --> file json
