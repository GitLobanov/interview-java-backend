# Docker-compose

```c
version: '3.9'

services:
    postgre_db:
        image: postgres:16.6
        container_name: postgre_db
        restart: unless-stopped
        environment:
            POSTGRES_DB: "postgres"
            POSTGRES_USER: "concifora"
            POSTGRES_PASSWORD: "g#5@/@52Dxx"
        volumes:
            - pg_data:/var/lib/postgresql/data
        ports:
            - "21002:5432"
        healthcheck:
            test: [ "CMD-SHELL", "pg_isready -U concifora -d postgres" ]
            interval: 15s
            timeout: 10s
            retries: 7
            start_period: 12s
        deploy:
            resources:
                limits:
                    cpus: '2'
                    memory: 4GB
        networks:
            - postgre-network
            - concifora-network

    pgadmin:
        image: dpage/pgadmin4:8.13.0
        container_name: pgadmin
        restart: unless-stopped
        environment:
            PGADMIN_DEFAULT_EMAIL: "god@concifora.com"
            PGADMIN_DEFAULT_PASSWORD: "pg@4c/%t)("
            PGADMIN_LISTEN_PORT: 80
        ports:
            - 22004:80
        volumes:
            - pgadmin_data:/var/lib/pgadmin
        depends_on:
            - postgre_db
        networks:
            - postgre-network
            - concifora-network

volumes:
    pg_data:
    pgadmin_data:


networks:
    postgre-network:
    concifora-network:

```

# Ресурсы

- [Установка и настройка PostgreSQL в Docker](https://timeweb.cloud/tutorials/postgresql/postgresql-docker-setup)
- [Быстрый запуск PostgreSQL через Docker Compose](https://habr.com/ru/articles/823816/)
- [postgres docker hub](https://hub.docker.com/_/postgres/tags?name=16)
- [Configure PostgreSQL and pgAdmin with Docker Compose](https://anasdidi.dev/articles/200713-docker-compose-postgres/#pgadmin)
- [dpage/pgadmin4 docker hub](https://hub.docker.com/r/dpage/pgadmin4)
- 
