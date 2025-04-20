# Вместе с Nginx Proxy Manager

```c
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

```c
docker-compose up -d
```

Default Admin User:

```c
Email:    admin@example.com
Password: changeme
```
# Ресурсы

- [Nginx Proxy Manager offical](https://nginxproxymanager.com/guide/)