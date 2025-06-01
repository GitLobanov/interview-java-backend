# Docker compose

```bash
services:
  keycloak:
    image: quay.io/keycloak/keycloak:26.1.0
    container_name: keycloak-service
    restart: unless-stopped
    command:
      - "start-dev"
    ports:
      - "22130:8443"
    environment:
      KC_HTTP_PORT: 8443
      KC_BOOTSTRAP_ADMIN_USERNAME: admin
      KC_BOOTSTRAP_ADMIN_PASSWORD: admin
      KC_DB: postgres
      KC_DB_URL_HOST: concifora.ru
      KC_DB_URL_PORT: 21020
      KC_DB_URL_DATABASE: KeyclokDB
      KC_DB_USERNAME: microservice
      KC_DB_PASSWORD: 123f.&msdg
      KC_HEALTH_ENABLED: "true"
      KC_PROXY_HEADERS: "xforwarded"
    healthcheck:
      test:
        [ "CMD-SHELL", "{ exec 3<>/dev/tcp/localhost/8443 && echo -e \"GET /health/ready HTTP/1.1\nhost: localhost:8443\n\" >&3 && timeout --preserve-status 1 cat <&3 | grep -m 1 -q 'status.*UP'; }" ]
      interval: 10s
      timeout: 5s
      start_period: 60s
      retries: 5
    networks:
      - concifora_default

networks:
  concifora_default:
```

# Resources

- [off site, getting started docker ](https://www.keycloak.org/getting-started/getting-started-docker)
- [off site, more with optimize](https://www.keycloak.org/server/containers)
- spring:  
  application:  
    name: cf-lang-api-gateway-service  
  #Security configuration  
  security:  
    oauth2:  
      client:  
        registration:  
          keycloak:  
            client-id: AdminUI  
            provider: keycloak  
            client-name: KEYCLOAK  
            client-secret: GwUn3t6il3ICw6cHtskHx0LtRfax0Dw7  
            scope: openid  
        provider:  
          keycloak:  
            issuer-uri: key.concifora.ru/realms/admin-ui