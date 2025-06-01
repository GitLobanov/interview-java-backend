## Готовые

### Котлин

```dockerfile
# Этап сборки JAR  
FROM gradle:7.6-jdk17 AS jar-builder  
WORKDIR /opt/build  
  
# Копируем файлы Gradle и загружаем зависимости  
COPY build.gradle.kts settings.gradle.kts ./  
COPY gradle ./gradle  
RUN gradle --no-daemon dependencies  
  
# Копируем исходный код и собираем JAR  
COPY src ./src  
RUN gradle --no-daemon build -x test  
  
# Этап создания компактного JAR  
FROM eclipse-temurin:17 AS compact-jar-builder  
WORKDIR /opt/build  
  
# Копируем JAR из предыдущего этапа  
COPY --from=jar-builder /opt/build/build/libs/*.jar ./application.jar  
  
# Извлекаем слои JAR с помощью Spring Boot Layertools  
RUN java -Djarmode=layertools -jar application.jar extract  
  
# Создаем минимальную версию JDK с помощью jlink  
RUN $JAVA_HOME/bin/jlink \  
         --add-modules `jdeps --ignore-missing-deps -q -recursive --multi-release 17 --print-module-deps -cp 'dependencies/BOOT-INF/lib/*':'snapshot-dependencies/BOOT-INF/lib/*' application.jar` \  
         --strip-debug \  
         --no-man-pages \  
         --no-header-files \  
         --compress=2 \  
         --output jdk  
  
# Финальный этап  
FROM debian:buster-slim  
  
ARG BUILD_PATH=/opt/build  
ENV JAVA_HOME=/opt/jdk  
ENV PATH "${JAVA_HOME}/bin:${PATH}"  
  
# Создаем пользователя для безопасности  
RUN groupadd --gid 1000 spring-app \  
  && useradd --uid 1000 --gid spring-app --shell /bin/bash --create-home spring-app  
  
USER spring-app:spring-app  
WORKDIR /opt/workspace  
  
# Копируем минимальный JDK и слои JAR  
COPY --from=compact-jar-builder $BUILD_PATH/jdk $JAVA_HOME  
COPY --from=compact-jar-builder $BUILD_PATH/spring-boot-loader/ ./  
COPY --from=compact-jar-builder $BUILD_PATH/dependencies/ ./  
COPY --from=compact-jar-builder $BUILD_PATH/snapshot-dependencies/ ./  
COPY --from=compact-jar-builder $BUILD_PATH/application/ ./  
  
# Запускаем приложение  
ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]
```

### Java simple

```dockerfile
# Этап сборки JAR  
FROM gradle:8.7-jdk17-alpine AS builder  
WORKDIR /app  
COPY . .  
RUN gradle --no-daemon build -Pversion=0.0.1-SNAPSHOT -x test  
#RUN echo "Listing files in /app/build/libs" && ls /app/build/libs  
  
FROM alpine/java:17-jre  
WORKDIR /app  
COPY --from=builder /app/build/libs/traston-auth-service-0.0.1-SNAPSHOT.jar /app/app.jar  
EXPOSE 9090  
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

### Java compact jar

```dockerfile
# Этап сборки JAR  
FROM gradle:8.7-jdk17-alpine AS builder  
WORKDIR /app  
COPY . .  
  
RUN gradle --no-daemon dependencies; gradle --no-daemon build -Pversion=0.0.1-SNAPSHOT -x test  
  
# Этап создания компактного JAR с использованием Spring Boot Layertools и минимизации JDK  
FROM eclipse-temurin:17 AS compact-jar-builder  
WORKDIR /opt/build  
  
COPY --from=builder /app/build/libs/*.jar ./application.jar  
  
RUN java -Djarmode=layertools -jar application.jar extract  
  
RUN $JAVA_HOME/bin/jlink \  
         --add-modules `jdeps --ignore-missing-deps -q -recursive --multi-release 17 --print-module-deps -cp 'dependencies/BOOT-INF/lib/*':'snapshot-dependencies/BOOT-INF/lib/*' application.jar` \  
         --strip-debug \  
         --no-man-pages \  
         --no-header-files \  
         --compress=2 \  
         --output jdk  
  
# Финальный этап - создание минимального контейнера с безопасностью  
FROM debian:buster-slim  
  
ARG BUILD_PATH=/opt/build  
ENV JAVA_HOME=/opt/jdk  
ENV PATH="${JAVA_HOME}/bin:${PATH}"  
  
RUN groupadd --gid 1000 spring-app \  
  && useradd --uid 1000 --gid spring-app --shell /bin/bash --create-home spring-app  
  
USER spring-app:spring-app  
WORKDIR /opt/workspace  
  
COPY --from=compact-jar-builder $BUILD_PATH/jdk $JAVA_HOME  
COPY --from=compact-jar-builder $BUILD_PATH/spring-boot-loader/ ./  
COPY --from=compact-jar-builder $BUILD_PATH/dependencies/ ./  
COPY --from=compact-jar-builder $BUILD_PATH/snapshot-dependencies/ ./  
COPY --from=compact-jar-builder $BUILD_PATH/application/ ./  
  
EXPOSE 9090  
  
ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]
```

