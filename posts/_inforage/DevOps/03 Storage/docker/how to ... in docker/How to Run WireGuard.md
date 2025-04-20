# Docker compose

```bash
services:
  
  wireguard:
    image: linuxserver/wireguard:v1.0.20210914-ls7
    container_name: wireguard
    cap_add:
      - NET_ADMIN
    volumes:
      - wireguard-config-data:/config
    ports:
      - "22110:5000"
      - "51820:51820/udp"
    restart: unless-stopped
    networks:
      - concifora_default

  wireguard-ui:
    image: ngoduykhanh/wireguard-ui:latest
    container_name: wireguard-ui
    depends_on:
      - wireguard
    cap_add:
      - NET_ADMIN
    # use the network of the 'wireguard' service. this enables to show active clients in the status page
    network_mode: service:wireguard
    environment:
      - SENDGRID_API_KEY
      - EMAIL_FROM_ADDRESS
      - EMAIL_FROM_NAME
      - SESSION_SECRET
      - WGUI_USERNAME=admin
      - WGUI_PASSWORD=admin
      - WG_CONF_TEMPLATE
      - WGUI_MANAGE_START=true
      - WGUI_MANAGE_RESTART=true
    logging:
      driver: json-file
      options:
        max-size: 50m
    volumes:
      - wireguard-db-data:/app/db
      - wireguard-config-data:/etc/wireguard
    restart: unless-stopped

networks:
  concifora_default:
    external: true
volumes:
  wireguard-config-data: {}
  wireguard-db-data: {}
```

# Resources

- [Installing WireGuard UI using Docker Compose](https://community.hetzner.com/tutorials/installing-wireguard-ui-using-docker-compose)
- [Set-up Wireguard and Wireguard-UI (GUI) on AWS: EC2](https://medium.com/@bertrandoubida/set-up-wireguard-and-wireguard-ui-gui-on-aws-ec2-50f552015296)
- [Wireguard — установка своего сервиса с помощью Docker](https://teletype.in/@dobriydenis/wireguard)
- 