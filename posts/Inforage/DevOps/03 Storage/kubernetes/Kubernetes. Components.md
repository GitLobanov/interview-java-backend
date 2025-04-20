![](Pasted%20image%2020250219142208.png)

## Master Node

**API Server:**

- Acts as the control plane for the Kubernetes cluster, serving the Kubernetes API.
- The central management point for the Kubernetes cluster.
- Exposes the Kubernetes API, which users, other components, and external tools interact with to control the cluster.

**etcd:**

- A distributed key-value store that stores configuration data.
- Stores configuration data and the current state of the cluster, providing a reliable and consistent storage solution.

**Controller Manager:**

- Manages controllers to regulate the state of the cluster.
- Examples include the Node Controller (ensures nodes are healthy), Replication Controller (maintains the desired number of replicas), and Endpoints Controller (populates Endpoints objects).

**Scheduler:**

- Assigns work (containers) to nodes based on resource requirements and constraints.
- Optimizes the allocation of resources across the cluster to ensure efficient utilization.

### Контроллеры

- **Deployment Controller**:
    - Отвечает за поддержание желаемого количества реплик подов, описанных в Deployment.
    - Если поды завершаются или удаляются, контроллер создаёт новые.    
- **ReplicaSet Controller**:
    - Управляет количеством реплик подов, описанных в ReplicaSet.
    - Обычно используется Deployment для управления ReplicaSet.
- **StatefulSet Controller**:
    - Управляет подами с уникальными идентификаторами и устойчивыми томами (например, для баз данных).    
- **Node Controller**:
    - Отслеживает состояние узлов (nodes) в кластере.
    - Если узел становится недоступным, контроллер помечает его как NotReady и перераспределяет поды на другие узлы.    
- **Service Controller**:
    - Управляет созданием и обновлением сервисов (Services), которые обеспечивают доступ к подам.
    - Например, создаёт LoadBalancer в облачных средах.
- **Endpoint Controller**:
    - Обновляет Endpoints (списки IP-адресов и портов подов) для сервисов.
- **Namespace Controller**:
    - Управляет жизненным циклом Namespace (создание, удаление).
- **Job Controller**:
    - Управляет задачами (Jobs), которые выполняются один или несколько раз.
- **DaemonSet Controller**:
    - Обеспечивает запуск пода на каждом узле (или на узлах, соответствующих определённым критериям). 
- **PersistentVolume Controller**:
    - Управляет жизненным циклом Persistent Volumes (PV) и Persistent Volume Claims (PVC).