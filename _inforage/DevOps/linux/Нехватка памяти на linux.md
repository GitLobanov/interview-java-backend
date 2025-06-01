## Linux

```
по дерикториям:
- sudo du -sh /* 2>/dev/null 
```

## Docker

```
sudo du -sh /var/lib/docker/overlay2/*/diff/tmp

глянуть логи для контейнера:
- du -sh /var/lib/docker/containers/*/*-json.log
глянуть занимаемое место докером:
- docker system df
- df -h /var/lib/docker

- docker inspect 68c2a9486290b60

sudo du -sh /var/lib/docker/overlay2/* | sort -h
sudo du -sh /var/lib/docker/*

for container_id in $(docker ps --format "{{.ID}}"); do
    merged_dir=$(docker inspect --format '{{.GraphDriver.Data.MergedDir}}' "$container_id")
    echo "Container ID: $container_id"
    echo "MergedDir: $merged_dir"
    echo "------------------------"
done

чистка логов:
- truncate -s 0 /var/lib/docker/containers/68c2a9486290b60a67b6305bb4670bda88c41642dd46c096f4cfd0fe5494f77f/68c2a9486290b60a67b6305bb4670bda88c41642dd46c096f4cfd0fe5494f77f-json.log
```

## Причины

- Если указывает на слои, возможно tmp, logs, topics
- захожим в контейнер кафки - /var/lib/kafka/data
- /var/log