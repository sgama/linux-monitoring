
# Linux Monitoring
## Install NodeExporter on your target(s)
```
* curl -LO https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz

* tar xvf node_exporter-0.18.1.linux-amd64.tar.gz
* sudo cp node_exporter-0.18.1.linux-amd64/node_exporter /usr/local/bin/
* sudo useradd --no-create-home --shell /bin/false node_exporter
* sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
* sudo vi /etc/systemd/system/node_exporter.service
<< Insert file found in systemctl/node_exporter.service here >>

* sudo systemctl daemon-reload
* sudo systemctl start node_exporter
* sudo systemctl enable node_exporter
```
## Configure Prometheus to scrape your target(s)
Edit `prometheus/prometheus.yml` to add your linux instance to the scrape config. A template is available for you with job name `my-linux-box`. Modify the targets array to include the IP Addresses of your box and the port in which prometheus can scrape the NodeExporter service.
```
- job_name: 'my-linux-box'
  static_configs:
    - targets: ['123.123.123.123:9100']
```
## Run the docker-compose stack
`docker-compose up -d`
## Verify the containers are running properly
`docker ps -a`
```
linux-monitoring> docker ps -a
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
7571a0ac669c prom/prometheus:latest "/bin/prometheus --c…" 6 seconds ago Up 5 seconds 0.0.0.0:9090->9090/tcp prometheus
a1a130fb0d4c grafana/grafana:latest "/run.sh" 6 seconds ago Up 5 seconds 0.0.0.0:3000->3000/tcp grafana
```
## Visit Grafana
Visit: `http://localhost:3000/login`
Auth: `admin:test`

Navigate to: `http://localhost:3000/d/nodeexporter/node-exporter?orgId=1&refresh=5s`
## Troubleshooting
If nothing shows up on the nodeexporter page, verify your targets are being scraped properly by visiting Prometheus: `http://localhost:9090/targets`
