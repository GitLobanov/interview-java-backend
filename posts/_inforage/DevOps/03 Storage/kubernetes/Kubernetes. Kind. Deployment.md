## Полный конфиг файл

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spring-boot-microservice
  labels:
    app: spring-boot-microservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: spring-boot-microservice
  template:
    metadata:
      labels:
        app: spring-boot-microservice
    spec:
      containers:
      - name: spring-boot-app
        image: your-docker-repo/spring-boot-microservice:latest
        ports:
        - containerPort: 8080
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: "prod"
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: username
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: password
        - name: APP_CONFIG
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: application.properties
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1"
        livenessProbe:
          httpGet:
            path: /actuator/health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /actuator/health
            port: 8080
          initialDelaySeconds: 15
          periodSeconds: 5
        startupProbe:
          httpGet:
            path: /actuator/health
            port: 8080
          failureThreshold: 30
          periodSeconds: 10
---
apiVersion: v1
kind: Service
metadata:
  name: spring-boot-microservice
  labels:
    app: spring-boot-microservice
spec:
  type: ClusterIP
  selector:
    app: spring-boot-microservice
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
---
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: spring-boot-microservice-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: spring-boot-microservice
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  application.properties: |
    spring.datasource.url=jdbc:mysql://mysql-service:3306/mydb
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    spring.jpa.hibernate.ddl-auto=update
---
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: dXNlcg== # base64-encoded "user"
  password: cGFzc3dvcmQ= # base64-encoded "password"
```

## Strategy

-	Rolling deployment – поочередное развертывание на разных серверах.

Это стандартная стратегия развертывания в Kubernetes. Она постепенно, один за другим, заменяет pod'ы со старой версией приложения на pod'ы с новой версией — без простоя кластера. Kubernetes дожидается готовности новых pod'ов к работе (проверяя их с помощью [readiness-тестов](https://www.weave.works/blog/resilient-apps-with-liveness-and-readiness-probes-in-kubernetes)), прежде чем приступить к сворачиванию старых.

```
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
       maxSurge: 25% # Максимальное количество дополнительных подов, которые могут быть созданы во время обновления. 
       # Например, при 3 репликах и maxSurge 25% может быть создано до 4 подов (3 + 25% от 3 = 3.75, округляется до 4).
       maxUnavailable: 25%  # Максимальное количество подов, которые могут быть недоступны во время обновления.
      # Например, при 3 репликах и maxUnavailable 25% может быть недоступно до 1 пода (25% от 3 = 0.75, округляется до 1). 
  template:
  ...
```

![](../../../../_res/Pasted%20image%2020250313120657.png)

-	Recreate deployment – удалить старую – вставить новую.

В этом простейшем типе развертывания старые pod'ы убиваются все разом и заменяются новыми.

```
spec:
  replicas: 3
  strategy:
    type: Recreate
  template:
  ...
```

![](../../../../_res/Pasted%20image%2020250313121418.png)

-	Blue-green deploy – 2 среды развертывания – green – основной рабочий проект, blue – новая версия, стратегия в том, что б начинать перенаправлять трафик на blue проект для его тестирования, если проект упадет – вернуть трафик на green, исправить blue, если blue cправляется успешно – заменить green – blue 

Стратегия сине-зеленого развертывания (иногда ее ещё называют red/black, т.е. красно-чёрной) предусматривает одновременное развертывание старой (зеленой) и новой (синей) версий приложения. После размещения обеих версий обычные пользователи получают доступ к зеленой, в то время как синяя доступна для QA-команды для автоматизации тестов через отдельный сервис или прямой проброс портов.

```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: awesomeapp-02
spec:
  template:
    metadata:
      labels:
        app: awesomeapp
        version: "02"
```

После того, как синяя (новая) версия была протестирована и был одобрен ее релиз, сервис переключается на неё, а зеленая (старая) сворачивается:

```
apiVersion: v1
kind: Service
metadata:
  name: awesomeapp
spec:
  selector:
    app: awesomeapp
    version: "02"
...
```

![](../../../../_res/Pasted%20image%2020250313121701.png)

-	Canary-deploy – предоставление новой версии для ограниченного кол-во пользователей.

![](../../../../_res/Pasted%20image%2020250313131209.png)

-	Feature toggled – внедрение нового функционала сразу в рабочую версию, при помощи флага функционал отключается до тех пор, пока не убедятся в его правильности.

С помощью переключателей функциональности _(feature toggles)_ и других инструментов можно следить за тем, как пользователи взаимодействуют с новой функцией, увлекает ли она их или они считают новый пользовательский интерфейс запутанным, и другими типами метрик.

-	Shadow deployment – теневое развертывание, использует 2 среды, первая основная, другая тестовая , запросы с основной дублируются на теневую для проверки работоспособности.

Разница между скрытым и канареечным развертыванием состоит в том, что скрытые развертывания имеют дело с фронтендом, а не с бэкендом, как канареечные.

![](../../../../_res/Pasted%20image%2020250313131710.png)