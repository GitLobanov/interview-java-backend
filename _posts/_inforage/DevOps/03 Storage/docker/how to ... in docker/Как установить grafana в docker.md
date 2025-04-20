# Docker-compose

```bash
services:
  grafana:
    image: grafana/grafana-enterprise
    container_name: grafana
    ports:
      - '22060:3000'
    restart: unless-stopped
    volumes:
      - grafana-data:/var/lib/grafana
    networks:
      - conscifora_default
    cpus: 2
    mem_limit: 2000m  
volumes:
  grafana-data: {}
networks:
  conscifora_default:
    external: true 
```

# Ресурсы

- [Официальная документация](https://grafana.com/docs/grafana/latest/administration/back-up-grafana/)
- [Тутор. Как запустить Grafana в контейнере Docker](https://itsecforu.ru/2022/03/21/%F0%9F%90%B3-%D0%BA%D0%B0%D0%BA-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D1%82%D0%B8%D1%82%D1%8C-grafana-%D0%B2-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%B5-docker/)
- 