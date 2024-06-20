# [Proxmox Exporter](https://github.com/prometheus-pve/prometheus-pve-exporter)

# Installing with pipenv on proxmox version 8

### Packages

```bash
apt install python3-pip python3-venv
```

### Directory creation

```bash
mkdir /opt/prometheus-pve-exporter
cd /opt/prometheus-pve-exporter/
```

### Enable venv and install requirements

```bash
python3 -m venv . 
source bin/activate
python3 -m pip install prometheus-pve-exporter
wget https://raw.githubusercontent.com/prometheus-pve/prometheus-pve-exporter/main/requirements.txt
pip install -r requirements.txt
```

# Authentication

Please see the official [documentation](https://github.com/prometheus-pve/prometheus-pve-exporter?tab=readme-ov-file#authentication)

```yaml
tee /opt/prometheus-pve-exporter/pve.yml<<EOF
default:
    user: prometheus@pve
    token_name: "your-token-id"
    token_value: "..."
EOF
```

# Systemd configuration

```bash
tee /etc/systemd/system/prometheus-pve-exporter.service<<EOF
[Unit]
Description=prometheus-pve-service

[Service]
User=root
WorkingDirectory=/opt/prometheus-pve-exporter
ExecStart=/opt/prometheus-pve-exporter/bin/python3 -m pve_exporter --config.file /opt/prometheus-pve-exporter/pve.yml

[Install]
WantedBy=multi-user.target
EOF
```

### Enable and start the systemd file

```bash
systemctl enable --now prometheus-pve-exporter.service
```

# Prometheus configuration

```yaml
scrape_configs:
  - job_name: 'pve'
    static_configs:
      - targets:
        - 192.168.1.2:9221  # Proxmox VE node with PVE exporter.
    metrics_path: /pve
    params:
      module: [default]
      cluster: ['1']
      node: ['1']
```