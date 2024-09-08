# Mysqld_exporter configure by Bash Script step by step.

# Blog Post: https://iamabhishek-dubey.medium.com/setting-up-mysql-monitoring-with-prometheus-6029cec87db0

sudo useradd --system  --no-create-home  --shell /bin/false mysqld_exporter
CREATE USER 'mysqld_exporter'@'localhost' IDENTIFIED BY 'password' WITH MAX_USER_CONNECTIONS 3;
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'mysqld_exporter'@'localhost';
wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.15.1/mysqld_exporter-0.15.1.linux-amd64.tar.gz
tar -xvf mysqld_exporter-0.15.1.linux-amd64.tar.gz
mv mysqld_exporter /usr/local/bin/

vim /etc/systemd/system/mysqld_exporter.service

```bash
# vim /etc/systemd/system/mysqld_exporter.service
[Unit]
Description=MySQL Exporter Service
Wants=network.target
After=network.target

[Service]
User=mysqld_exporter
Group=mysqld_exporter
Environment="DATA_SOURCE_NAME=mysqld_exporter:password@unix(/var/run/mysqld/mysqld.sock)"
Type=simple
ExecStart=/usr/local/bin/mysqld_exporter   --config.my-cnf=/etc/.mysqld_exporter.cnf
Restart=always

[Install]
WantedBy=multi-user.target

```

vim /etc/.mysqld_exporter.cnf
```bash
# /etc/.mysqld_exporter.cnf
[client]
user=mysqld_exporter
password=password
host=localhost
```


vim /etc/prometheus/prometheus.yml
```bash
    - job_name: "mysqld_exporter"
        static_configs:
        - targets:
          - <mysql_ip>:9104
```

systemctl restart prometheus
