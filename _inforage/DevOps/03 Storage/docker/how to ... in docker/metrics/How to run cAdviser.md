# Docker compose

```
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
```