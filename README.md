<h1 align="center" style="border-bottom: none; display: flex;">
    <div><a href="//prometheus.io" target="_blank"><img alt="Prometheus" src="/prometheus-logo.svg"></a>Prometheus</div>
    <div><a href="https://grafana.com/" target="_blank"><img alt="Grafana" src="/grafana-logo.png"></a></div>
</h1>

## Architecture overview

![Architecture overview](/architecture.svg)

## Install Prometheus

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.50.0-rc.0/prometheus-2.50.0-rc.0.linux-amd64.tar.gz
```
```bash
tar xvf prometheus-2.50.0-rc.0.linux-amd64.tar.gz
```
```bash
sudo groupadd --system prometheus
```
```bash
sudo useradd --system -s /sbin/nologin -g prometheus prometheus
```
```bash
sudo mv prometheus promtool /usr/local/bin/
```
```bash
sudo mkdir /etc/prometheus
```
```bash
sudo mkdir /var/lib/prometheus
```
```bash
sudo chown -R prometheus:prometheus /var/lib/prometheus
```
```bash
sudo mv consoles/ console_libraries/ prometheus.yml /etc/prometheus/
```
```bash
cd /etc/prometheus/
```
```bash
sudo nano prometheus.yml
```
```bash
sudo nano /etc/systemd/system/prometheus.service
```
```bash
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl daemon-reload
```
```bash
sudo systemctl status prometheus
```
```bash
sudo systemctl enable --now prometheus
```
```bash
sudo systemctl status prometheus.service
```
```bash
sudo lsof -n -i | grep prometheus
```
```bash
hostname -I
```
```bash
http://172.18.231.121:9090/targets?search=
```

## Install Node Exporter

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
```
```bash
tar xvf node_exporter-1.7.0.linux-amd64.tar.gz
```
```bash
cd node_exporter-1.7.0.linux-amd64/
```
```bash
sudo mv node_exporter /usr/local/bin/
```
```bash
node_exporter --version
```
```bash
sudo nano /etc/systemd/system/node-exporter.service
```
```bash
[Unit]
Description=Prometheus exporter for machine metrics

[Service]
Restart=always
User=prometheus
ExecStart=/usr/local/bin/node_exporter
ExecReload=/bin/kill -HUP $MAINPID
TimeoutStopSec=20s
SendSIGKILL=no

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl daemon-reload
```
```bash
sudo systemctl status node-exporter.service
```
```bash
sudo systemctl enable --now node-exporter.service
```
```bash
sudo systemctl status node-exporter.service
```
```bash
sudo lsof -n -i | grep node
```
```bash
hostname -I
```
```bash
http://172.18.231.121:9100/metrics
```
```bash
sudo nano /etc/prometheus/prometheus.yml
```
```bash
# lakukan perubahan di prometheus.yml
 - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "node-exporter"
    static_configs:
      - targets: ["localhost:9100"]
```
```bash
sudo systemctl restart prometheus
```
```bash
sudo systemctl status prometheus
```

## Install Node Exporter

```bash
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```
```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```
```bash
sudo apt update
```
```bash
sudo apt install grafana
```
```bash
sudo systemctl status grafana-server.service
```
```bash
sudo systemctl enable --now grafana-server.service
```
```bash
sudo systemctl status grafana-server.service
```
```bash
sudo lsof -n -P -i | grep grafana
```
```bash
hostname -I
```
```bash
http://172.18.231.121:3000/login
```
```bash
username : admin
password : admin
```
```bash
masukkan connection : http://localhost:9090
add dashboard
```
```bash
untuk memunculkan dashboard secara full 
follow ini https://grafana.com/grafana/dashboards/1860-node-exporter-full/
selanjutnya copy dashboard id
kemudian klik import
```