<h1 align="center" style="border-bottom: none">
    <a href="//prometheus.io" target="_blank"><img alt="Prometheus" src="/prometheus-logo.svg"></a><br>Prometheus
</h1>

<p align="center">Visit <a href="//prometheus.io" target="_blank">prometheus.io</a> for the full documentation,
examples and guides.</p>

<div align="center">

[![CI](https://github.com/prometheus/prometheus/actions/workflows/ci.yml/badge.svg)](https://github.com/prometheus/prometheus/actions/workflows/ci.yml)
[![Docker Repository on Quay](https://quay.io/repository/prometheus/prometheus/status)][quay]
[![Docker Pulls](https://img.shields.io/docker/pulls/prom/prometheus.svg?maxAge=604800)][hub]
[![Go Report Card](https://goreportcard.com/badge/github.com/prometheus/prometheus)](https://goreportcard.com/report/github.com/prometheus/prometheus)
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/486/badge)](https://bestpractices.coreinfrastructure.org/projects/486)
[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/prometheus/prometheus)
[![Fuzzing Status](https://oss-fuzz-build-logs.storage.googleapis.com/badges/prometheus.svg)](https://bugs.chromium.org/p/oss-fuzz/issues/list?sort=-opened&can=1&q=proj:prometheus)
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/prometheus/prometheus/badge)](https://securityscorecards.dev/viewer/?uri=github.com/prometheus/prometheus)

</div>

Prometheus, a [Cloud Native Computing Foundation](https://cncf.io/) project, is a systems and service monitoring system. It collects metrics
from configured targets at given intervals, evaluates rule expressions,
displays the results, and can trigger alerts when specified conditions are observed.

The features that distinguish Prometheus from other metrics and monitoring systems are:

* A **multi-dimensional** data model (time series defined by metric name and set of key/value dimensions)
* PromQL, a **powerful and flexible query language** to leverage this dimensionality
* No dependency on distributed storage; **single server nodes are autonomous**
* An HTTP **pull model** for time series collection
* **Pushing time series** is supported via an intermediary gateway for batch jobs
* Targets are discovered via **service discovery** or **static configuration**
* Multiple modes of **graphing and dashboarding support**
* Support for hierarchical and horizontal **federation**

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