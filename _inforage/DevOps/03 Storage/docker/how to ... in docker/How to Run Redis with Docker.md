# Commands

Можно задать количество реплик через scale

```bash
docker-compose up -d --scale redis-master=1 --scale redis-replica=1
```

# Docker-compose | master, replica, ui insight

```bash
services:
    redis-master:
        image: 'bitnami/redis:latest'
        container_name: redis-master
        restart: unless-stopped
        ports:
            - '21031:6379'
        environment:
            - REDIS_REPLICATION_MODE=master
            - REDIS_PASSWORD=master_fackt@228.ru
        volumes:
            - redis-storage:/bitnami
        networks:
            - conscifora_default    
    redis-replica:
        image: 'bitnami/redis:latest'
        ports:
            - '21032:6379'
        depends_on:
            - redis-master
        environment:
            - REDIS_REPLICATION_MODE=slave
            - REDIS_MASTER_HOST=redis-master
            - REDIS_MASTER_PORT_NUMBER=6379
            - REDIS_MASTER_PASSWORD=master_fackt@228.ru
            - REDIS_PASSWORD=fackting_slave@228.ru
        networks:
            - conscifora_default
    redis-insight:
        image: redislabs/redisinsight:latest
        container_name: redis-insight
        ports:
            - '22007:5540'
        volumes:
            - redis-insight-storage:/data
        networks:
            - conscifora_default
volumes:
    redis-storage: {}
    redis-insight-storage: {}
networks:
    conscifora_default:
        external: true
```


# Docker-compose | master, 3 replica, ui insight

```bash
services:
    redis-master:
        image: 'bitnami/redis:latest'
        container_name: redis-master
        restart: unless-stopped
        ports:
            - '21080:6379'
        environment:
            - REDIS_REPLICATION_MODE=master
            - REDIS_PASSWORD=master_fackt@228.ru
        volumes:
            - redis-master-data:/bitnami
        networks:
            - concifora_default    
    

    redis-replica-1:
        image: 'bitnami/redis:latest'
        container_name: redis-replica-1
        ports:
        - '21081:6379'
        depends_on:
        - redis-master
        environment:
        - REDIS_REPLICATION_MODE=slave
        - REDIS_MASTER_HOST=redis-master
        - REDIS_MASTER_PORT_NUMBER=6379
        - REDIS_MASTER_PASSWORD=master_fackt@228.ru
        - REDIS_PASSWORD=fackting_slave@228.ru
        volumes:
        - redis-replica-data-1:/bitnami
        networks:
        - concifora_default

    redis-replica-2:
        image: 'bitnami/redis:latest'
        container_name: redis-replica-2
        ports:
        - '21082:6379'
        depends_on:
        - redis-master
        environment:
        - REDIS_REPLICATION_MODE=slave
        - REDIS_MASTER_HOST=redis-master
        - REDIS_MASTER_PORT_NUMBER=6379
        - REDIS_MASTER_PASSWORD=master_fackt@228.ru
        - REDIS_PASSWORD=fackting_slave@228.ru
        volumes:
        - redis-replica-data-2:/bitnami
        networks:
        - concifora_default

    redis-replica-3:
        image: 'bitnami/redis:latest'
        container_name: redis-replica-3
        ports:
        - '21083:6379'
        depends_on:
        - redis-master
        environment:
        - REDIS_REPLICATION_MODE=slave
        - REDIS_MASTER_HOST=redis-master
        - REDIS_MASTER_PORT_NUMBER=6379
        - REDIS_MASTER_PASSWORD=master_fackt@228.ru
        - REDIS_PASSWORD=fackting_slave@228.ru
        volumes:
        - redis-replica-data-3:/bitnami
        networks:
        - concifora_default

    redis-insight:
        image: redislabs/redisinsight:latest
        container_name: redis-insight
        ports:
            - '22080:5540'
        volumes:
            - redis-insight-data:/data
        networks:
            - concifora_default
volumes:
    redis-master-data: {}
    redis-insight-data: {}
    redis-replica-data-1:
    redis-replica-data-2:
    redis-replica-data-3:
networks:
    concifora_default:
        external: true
```

# Ресурсы

- [How to Run Redis with Docker Compose Tutorial](https://cloudinfrastructureservices.co.uk/run-redis-with-docker-compose/)
- [Redis Insight](https://collabnix.com/running-redisinsight-using-docker-compose/)
- [Official Redis Website | Redis Insight](https://redis.io/docs/latest/operate/redisinsight/install/install-on-docker/)
- [Redis Cluster Using Docker](https://bytegoblin.io/blog/redis-cluster-using-docker)
- 