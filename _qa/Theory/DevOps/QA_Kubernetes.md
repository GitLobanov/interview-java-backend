## QA

#### Что такое Node?

- Физический или виртуальный сервер, на котором работают Pod’ы.
- Рабочая лошадка кластера: выполняет задачи, выделенные Control Plane.
####  Типы Node

1. Worker Node (обычные узлы):
    - Запускают пользовательские Pod’ы.
2. Master/Control Plane Node (если нет отдельного Control Plane):
    - Запускают системные Pod’ы (`kube-apiserver`, `etcd` и др.).
#### Какие основные компоненты master node в Kubernetes?

- etcd — распределённое хранилище, которое хранит конфигурации и состояние кластера.
- API server — это точка взаимодействия для всех запросов в кластер, предоставляет REST API.
- Scheduler — назначает поды на рабочие узлы, основываясь на доступных ресурсах и других факторах.
- Controller Manager — следит за состоянием системы и корректирует его, если оно не соответствует желаемому состоянию, используя контроллеры, такие как Deployment, ReplicaSet и т. д.

[Kubernetes. Components](../../../_inforage/DevOps/kubernetes/Kubernetes.%20Components.md)

#### Основные компоненты worker node

Рабочий узел (node) — это сервер (виртуальная машина, физический сервер и т. д.), который запускает приложения с помощью Pod и управляется главным узлом. Pods планируются на рабочих узлах, которые имеют необходимые инструменты для их запуска и подключения. Pod — это единица планирования в Kubernetes. Это логическая коллекция одного или нескольких контейнеров, которые всегда планируются вместе.

Рабочий узел имеет следующие компоненты:

- Container runtime
- kubelet
- kube-proxy

#### Что такое под?

- Минимальная deploy-единица в Kubernetes.
- Может содержать один или несколько контейнеров, которые:
    - Разделяют общие сетевые пространства (один IP).
    - Имеют общие тома (volumes).
    - Запускаются и останавливаются вместе.
        
Особенности Pod

- **Временные сущности** (ephemeral) – могут быть пересозданы в любой момент.
- Каждый Pod имеет **уникальный IP** в кластере (но меняется при пересоздании).
- Контейнеры внутри Pod общаются через `localhost`.
- Обычно **один Pod = одно приложение** (но бывают мультиконтейнерные Pod’ы, например, sidecar-паттерн).
    
Жизненный цикл Pod

1. **Pending** – создается, но контейнеры еще не запущены.
2. **Running** – хотя бы один контейнер работает.
3. **Succeeded/Failed** – все контейнеры завершились (успешно или с ошибкой).
4. **Terminating** – Pod удаляется.
    
Основные компоненты Pod (для схемы)

- **Контейнер(ы)** (`containers`) – основное приложение (например, `nginx`).
- **Тома (`volumes`)** – общее хранилище для контейнеров.
- **Метки (`labels`)** – например, `app: frontend`.
- **Environment Variables** – переменные окружения.
- **Probes** (`liveness`, `readiness`) – проверки здоровья.
- **Init-контейнеры** (если есть) – запускаются до основного контейнера.

Пример:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
  volumes:
  - name: shared-data
    emptyDir: {}
```
#### Зачем master node нужен Scheduler?

- Для  распределения подов (pods) по рабочим узлам (worker nodes)
- Например VPA изменил требуемые ресурсы для пода, и решил пере деплоить их. Шедулер направляет в каких нодах лучше развернуть, чтобы было достаточно под новые требования 
- Когда поды требуют больше CPU или памяти, чем имеется на текущем узле, Scheduler действительно ищет узлы с подходящими характеристиками, чтобы разместить их там. Кроме того, он учитывает такие параметры, как доступность ресурсов, ограничения на узлы и т. д.

###### Что такое менеджер контроллеров?

**Controller Manager** — это компонент мастер-ноды Kubernetes, который управляет набором **контроллеров**. Эти контроллеры следят за состоянием кластера и приводят его к желаемому состоянию, описанному в манифестах (например, Deployment, StatefulSet, Service и т.д.).

[Контроллеры](../../../_inforage/DevOps/kubernetes/Kubernetes.%20Components.md#Контроллеры)
###### Что такое kubelet и какую функцию он выполняет?

**Kubelet** — это агент, который работает на каждом рабочем узле (worker node) и взаимодействует с главным узлом (master). Заместитель мастера, следит на воркер нодой. Он отвечает за запуск контейнеров внутри подов на рабочем узле и за следование тому состоянию, которое описано в манифестах.

Kubelet подключается к среде выполнения контейнера с помощью _Container Runtime Interface_ (CRI). Интерфейс среды выполнения контейнера состоит из буферов протоколов, gRPC AP и библиотек.
###### Для чего нужен kube-proxy?

- **Сетевую коммуникацию** между сервисами и подами.
- направлять трафик к нужному поду, если он находится на этой ноде;
- или перенаправить трафик на другую ноду, где есть нужный под (через iptables или IPVS).
- Когда вы создаете сервис типа `ClusterIP`, kube-proxy настраивает правила, чтобы трафик на этот IP и порт перенаправлялся к одному из подов, связанных с сервисом.

###### Какие типы сервисов существуют в Kubernetes?

**ClusterIP** (по умолчанию):
    - Виден только внутри кластера.
    - Назначается внутренний IP-адрес.
**NodePort**:    
    - Открывает статический порт на каждой ноде кластера.
    - Доступен извне через `<NodeIP>:<NodePort>`.
**LoadBalancer**:    
    - Интегрируется с облачным провайдером (AWS, GCP) для создания внешнего балансировщика нагрузки.
    - Автоматически настраивает `NodePort` и `ClusterIP`.
**ExternalName**:    
    - Сопоставляет сервис с DNS-именем (например, `my-service → api.external.com`).
    - Не имеет селекторов и Endpoints.

###### В чем разница между Service Mesh и API Gateway?

![](../../../_res/Pasted%20image%2020250213101424.png)

###### Что такое Istio и Consul?

- **Istio**:
    - **Service Mesh** для Kubernetes и других платформ.
    - Основные компоненты: **Envoy** (sidecar-прокси), **Pilot** (управление конфигурацией), **Citadel** (безопасность), **Galley** (валидация).
    - Функции: управление трафиком, mTLS, метрики, трассировка через Jaeger, Canary-развертывания.
        
- **Consul**:
    - Продукт **HashiCorp**, сочетающий **Service Mesh** и **Service Discovery**.
    - Работает в гибридных средах (Kubernetes, VM, bare-metal).
    - Использует **Connect** для mTLS и авторизации, поддерживает multiple DC.
    - Интегрируется с Kubernetes через Consul Helm-чарт.

#### Какие существуют стратегии деплоя в Kubernetes?

-	Blue-green deploy – 2 среды развертывания – green – основной рабочий проект, blue – новая версия, стратегия в том, что б начинать перенаправлять трафик на blue проект для его тестирования, если проект упадет – вернуть трафик на green, исправить blue, если blue cправляется успешно – заменить green – blue 
-	Canary-deploy – предоставление новой версии для ограниченного кол-во пользователей.
-	Rolling deployment – поочередное развертывание на разных серверах.
-	Feature toggled – внедрение нового функционала сразу в рабочую версию, при помощи флага функционал отключается до тех пор, пока не убедятся в его правильности.
-	Shadow deployment – теневое развертывание, использует 2 среды, первая основная, другая тестовая , запросы с основной дублируются на теневую для проверки работоспособности.
-	Recreate deployment – удалить старую – вставить новую.

[Kubernetes. Kind. Deployment](../../../_inforage/DevOps/kubernetes/Kubernetes.%20Kind.%20Deployment.md)

#### Что такое probes?

|Probe|Цель|Если проверка провалится|Пример использования|
|---|---|---|---|
|**Startup**|Дождаться старта приложения.|Контейнер перезапустится.|Приложения с долгой инициализацией.|
|**Liveness**|Убедиться, что контейнер не завис.|Контейнер перезапустится.|Критичный сервис (например, API).|
|**Readiness**|Проверить, может ли контейнер принимать запросы.|Контейнер исключается из балансировки трафика.|Базы данных, очереди (например, Redis).|

Все три типа проверок (`liveness`, `readiness`, `startup`) работают по одной логике:

1. Задержка перед стартом (`initialDelaySeconds`) – Kubernetes ждёт указанное время перед первой проверкой.
2. Периодичность (`periodSeconds`) – как часто повторять проверку.
3. Порог ошибок (`failureThreshold`) – сколько раз подряд проверка должна провалиться, чтобы Kubernetes принял меры.
4. Порог успеха (`successThreshold`) – сколько раз подряд проверка должна пройти для успешного статуса (актуально для `readiness` после fail).

Startup Probe (Стартовая проверка)

```yaml
startupProbe:
  httpGet:
    path: /init-status
    port: 8080
  failureThreshold: 30  # 30 попыток
  periodSeconds: 5      # Проверяем каждые 5 сек (макс. 150 сек на старт)
```

Liveness Probe (Живучесть)

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 15  # Ждём 15 сек после старта контейнера
  periodSeconds: 10        # Проверяем каждые 10 сек
  failureThreshold: 3      # 3 провала подряд = контейнер "мёртв"
```

 Readiness Probe (Готовность)

```yaml
readinessProbe:
  exec:
    command: ["pg_isready", "-U", "postgres"]
  initialDelaySeconds: 5
  periodSeconds: 5
  successThreshold: 1  # 1 успешная проверка = контейнер готов
```
#### Что нужно знать о кубере разработчику?

- **Во-первых**, как работает Kubernetes для того, чтобы запустить в нём приложение. То есть, что происходит в кластере, когда разработчик делает `kubectl apply -f deploy.yaml`. Например, почему деплой прошел, а Pod`ы находятся в статусе pending? В таком случае нет смысла смотреть controller-manager. Можно посмотреть в describe Deployment`а и увидеть, что там создался ReplicaSet. Вот знать не необходимо, но это очень хорошо позволяет понимать, что происходит с кластером Kubernetes и почему твои приложения запускаются или не запускаются или запускаются долго и так далее.
- **Во-вторых**, хорошо бы, но не обязательно знать об инфраструктурных ограничениях, которые есть в кластере. О том, что там есть ресурсы, CPU, память, что эти ресурсы могут ограничивать админы. Что есть такие объекты как Limit Range и Resource Quota, их админы применяют, чтобы ограничивать ресурсы и разделять разделять ресурсы по пространствам имен (namespace).
- **Дальше**: разработчику тоже хорошо бы знать, что есть в кластере процессы аутентификации и авторизации, что есть такая штука как RBAC (Role-Based Access Control). Все это используется админами, чтобы разделять права в кластере. Что вообще в кластере есть какие-то права.
- Основные абстракции которые есть в Kubernetes: Pod, Deployment, ReplicaSet, StatefulSet, Job, CronJob. Это всё нужно просто потому, что это то, что описывает твое приложение в рамках кластера Kubernetes.
###### Что такое Resource Quota

- Resource Quota позволяет задать максимальное количество ресурсов (например, CPU, памяти), которые могут быть использованы в рамках одного пространства имён.
- Можно ограничить количество создаваемых объектов определённого типа (например, поды, сервисы, PersistentVolumeClaims).
- Resource Quota защищает кластер от ситуаций, когда одно приложение или команда монополизирует все доступные ресурсы.

Пример использование:

Вы хотите ограничить использование ресурсов для команды разработки в пространстве имён `dev-team`. Команда может использовать максимум 4 CPU, 8 ГБ памяти и создать до 10 подов.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev-team
spec:
  hard:
    requests.cpu: "4"
    requests.memory: "8Gi"
    limits.cpu: "8"
    limits.memory: "16Gi"
    pods: "10"
```

###### Что такое RBAC Kubernetes

RBAC работает по принципу "проверки разрешений". Когда пользователь или сервис пытается выполнить действие (например, создать под), Kubernetes проверяет:

1. Имеет ли субъект (пользователь или сервисный аккаунт) соответствующие права через RoleBinding или ClusterRoleBinding.
2. Разрешает ли роль (Role или ClusterRole) выполнение этого действия.

Примеры конфигурации RBAC

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev-team
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: dev-team
subjects:
- kind: User
  name: alice
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin-role
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["*"]
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-binding
subjects:
- kind: User
  name: bob
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-admin-role
  apiGroup: rbac.authorization.k8s.io
```
###### Что такое Pod Disruption Budget.

Если PDB требует, чтобы хотя бы 3 пода из 5 были доступны, то Kubernetes не позволит удалить более 2 подов одновременно.

Сценарий 1: Гарантия минимум 3 работающих подов

```java
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: pdb-min-available
spec:
  minAvailable: 3
  selector:
    matchLabels:
      app: web
```

Этот PDB гарантирует, что минимум 3 пода с меткой `app: web` будут доступны.
###### Что поднимает контейнеры в подах?

Контейнеры в подах Kubernetes запускаются и управляются с помощью **Kubelet** — одного из ключевых компонентов Kubernetes

Kubernetes не работает напрямую с контейнерами, такими как Docker, containerd или CRI-O. Вместо этого он использует стандартный интерфейс **CRI (Container Runtime Interface)** , который позволяет взаимодействовать с различными runtime через единый протокол.

###### Что такое Stateful Set

- Уникальность и стабильность каждого Pod’а (постоянные имя, hostname и порядок развёртывания).
- Привязку к постоянному хранилищу (PersistentVolume) для каждого экземпляра.
- Гарантированный порядок запуска/остановки (например, `pod-0` → `pod-1` → `pod-2`).

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 10Gi
```

Когда использовать?

- Базы данных (MySQL, PostgreSQL, MongoDB).
- Кластерные приложения (Zookeeper, Elasticsearch).
- Любой сервис, требующий постоянства данных и идентификаторов.
    
Отличие от Deployment:

- Deployment — для stateless-приложений (все Pod’ы идентичны).
- StatefulSet — для stateful-приложений (каждый Pod уникален и имеет своё хранилище).
#### Что такое HPA

**HPA (Horizontal Pod Autoscaler)** в Kubernetes — это механизм автоматического масштабирования количества Pod’ов в Deployment/ReplicaSet на основе текущей нагрузки (CPU, памяти или кастомных метрик), чтобы обеспечить стабильную работу приложения без избыточного потребления ресурсов.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

#### Что такое VPA

**VPA (Vertical Pod Autoscaler)** в Kubernetes — это механизм автоматической настройки **requests** и **limits** CPU/памяти для Pod’ов на основе исторического потребления ресурсов, чтобы оптимизировать их использование без ручного вмешательства.

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: my-app-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind: Deployment
    name: my-app
  updatePolicy:
    updateMode: "Auto"
```
## Resources

- [Just-in-Time Kubernetes: Руководство начинающим для понимания основных концепций Kubernetes](https://habr.com/ru/companies/otus/articles/650231/)
- [Kubernetes для разработчиков | Илья Бочаров](https://www.youtube.com/watch?v=9ZxZ9gb9pQs)
- [Круглый стол «Нужно ли разработчику знать Kubernetes»](https://www.youtube.com/watch?v=XAuXswdHi5U&t=4s)
