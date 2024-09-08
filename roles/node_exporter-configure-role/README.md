# Node_exporter configure by Bash Script Step by step:

sudo useradd --system  --no-create-home  --shell /bin/false node_exporter
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
tar xvfz node_exporter-*.*-amd64.tar.gz
sudo mv node_exporter-1.8.2.linux-amd64/node_exporter /usr/local/bin/
rm -rf node_exporter*
node_exporter --version


sudo vim /etc/systemd/system/node_exporter.service
#vim /etc/systemd/system/node_exporter.service

[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter \
    --collector.logind

[Install]
WantedBy=multi-user.target
```

sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter


sudo vim /etc/prometheus/prometheus.yml

```bash
...
  - job_name: node_export
    static_configs:
      - targets: ["localhost:9100/metrics"]
```





        # Proxy requests to /metrics to Grafana server
        location /metrics/ {
                proxy_pass http://127.0.0.1:9100/;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }