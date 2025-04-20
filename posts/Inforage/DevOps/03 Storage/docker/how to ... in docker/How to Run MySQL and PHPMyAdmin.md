# Docker compose

```bash
services:
  mysql:
    image: mysql:latest
    container_name: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: ghbdtn!z@gfhjkm#
      MYSQL_DATABASE: mysql
      MYSQL_USER: microservice
      MYSQL_PASSWORD: kjfrkmysq.pth
    volumes:
      - mysql_data:/var/lib/mysql
    ports:
      - "21090:3306"
    networks:
      - concifora_default  

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    container_name: phpmyadmin
    restart: always
    depends_on:
      - mysql
    environment:
      PMA_HOST: mysql
      MYSQL_ROOT_PASSWORD: ghbdtn!z@gfhjkm#
    volumes:
    - phpmyadmin-sessions:/sessions
    ports:
      - "22090:80"
    networks:
      - concifora_default   


volumes:
    mysql_data:
    phpmyadmin-sessions:
networks:
    concifora_default:
        external: true      
```

# Resourses

- [Setting Up MySQL and phpMyAdmin with Docker Compose](https://medium.com/@elaurichetoho/setting-up-mysql-and-phpmyadmin-with-docker-compose-e569881d394e)
- 