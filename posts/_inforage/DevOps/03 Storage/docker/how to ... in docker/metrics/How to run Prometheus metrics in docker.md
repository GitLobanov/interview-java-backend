---
metrics:
  - cAdvisor
  - node_exporter
  - prometheus
---
# prometheus.yml

```bash
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
#           - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"
    static_configs:
      - targets: ['prometheus:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['194.226.49.251:21050']

  - job_name: 'kafka-exporter'
    static_configs:
      - targets: ['194.226.49.251:21032']

  - job_name: 'cadvisor-exporter'
    scrape_interval: '5s'
    static_configs:
      - targets: ['194.226.49.251:21051']

  # - job_name: 'a-tink_backend_service'
  #   scrape_interval: 5s
  #   metrics_path: '/actuator/prometheus'
  #   static_configs: 
  #     - targets: ['$CI_SERVER_HOST:8081']
```

# Docker-compose

```bash
services:
  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./prom-conf:/etc/prometheus/
      - prometheus-data:/prometheus
    container_name: prometheus
    restart: unless-stopped
    ports:
      - 21051:9090
    cpus: 1
    mem_limit: 1000m
    networks:
      - conscifora_default

  node-exporter:
    image: quay.io/prometheus/node-exporter:latest
    container_name: node_exporter
    command:
      - '--path.rootfs=/host'
    pid: host
    ports:
      - "21050:9100"
    restart: unless-stopped
    volumes:
      - node-exporter-data:/host:ro,rslave
    cpus: 0.5
    mem_limit: 500m
    networks:
      - concifora_default

  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    container_name: cadvisor_exporter
    ports:
      - "21051:8080"
    restart: unless-stopped
    volumes:
    - /:/rootfs:ro
    - /var/run:/var/run:rw
    - /sys:/sys:ro
    - /var/lib/docker/:/var/lib/docker:ro
    cpus: 0.5
    mem_limit: 500m
    networks:
      - concifora_default

volumes:
  prometheus-data:
    driver: local
  node-exporter-data:
    driver: local  
networks:
  conscifora_default:
    external: true
```

# Resources

- [With Alertmanager](https://www.dmosk.ru/miniinstruktions.php?mini=prometheus-stack-docker)
- [Docker compose and grafana](https://sahsumit.medium.com/server-monitoring-using-prometheus-and-grafana-a041b3333fa7)
- [Monitoring Docker container metrics using cAdvisor](https://prometheus.io/docs/guides/cadvisor/)
- 