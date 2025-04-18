# Stat usage

![](Pasted%20image%2020250119125104.png)

Примерно 2,5 ГБ без нагрузок, голый запущенный контейнер.
# Docker compose

```bash
services:
  sonarqube:
    image: sonarqube:community
    container_name: sonar-service
    ports:
      - "21140:9000"
      - "21141:9092"
    environment:
      - SONARQUBE_JDBC_URL=jdbc:postgresql://concifora.ru:21020/SonarDB
      - SONARQUBE_JDBC_USERNAME=sonar
      - SONARQUBE_JDBC_PASSWORD=sonar
    networks:
      - concifora_default
networks:
  concifora_default:
    external: true
```

# Maven 

```bash
mvn clean verify sonar:sonar
```

```xml
<profiles>  
    <profile>  
        <id>sonar</id>  
        <activation>  
            <activeByDefault>true</activeByDefault>  
        </activation>  
        <properties>  
            <sonar.host.url>  
                http://concifora.ru:21140  
            </sonar.host.url>  
            <sonar.token>  
                sqp_df05d1a5259817d385cc28ef8f953b48f1329cec  
            </sonar.token>  
            <sonar.projectKey>  
                test_project  
            </sonar.projectKey>  
            <sonar.projectName>  
                test_project  
            </sonar.projectName>  
        </properties>  
    </profile>  
</profiles>
```
# Resources

- [Установка SonarQube с использованием docker](https://voblachke.ru/blog/ustanovka-sonarqube-s-ispolzovaniem-docker-compose-na-windows/)
- 