### Документные базы данных (хранят данные в виде JSON, BSON, XML)

- **MongoDB** – самая популярная документная NoSQL БД, поддерживает гибкие схемы данных.
- **CouchDB** – использует JSON-документы и репликацию, оптимизирована для распределённых систем.
- **Firebase Firestore** – облачная NoSQL база от Google, активно используется в мобильных и веб-приложениях.

### Ключ-значение (Key-Value) базы данных (быстрый доступ по ключу)

- **Redis** – сверхбыстрая in-memory NoSQL база, часто используется для кеширования.
- **Amazon DynamoDB** – облачная NoSQL база от AWS, хорошо масштабируется.
- **Riak KV** – распределённая база данных с высокой отказоустойчивостью.

### Графовые базы данных (оптимизированы для работы с графами и связями)

- **Neo4j** – самая популярная графовая база, используется в социальных сетях, рекомендательных системах.
- **ArangoDB** – мультипарадигменная база (графы + документы + ключ-значение).  
    Amazon Neptune – облачная графовая БД от AWS, поддерживает Gremlin и SPARQL.

### Колонночные базы данных (работают с огромными объёмами данных, оптимизированы для аналитики)

- **Apache Cassandra** – масштабируемая распределённая NoSQL БД от Facebook.
- **HBase** – построена на Hadoop, подходит для Big Data.
- **Google Bigtable** – облачная колонночная база от Google.

### Time-Series базы данных (оптимизированы для работы с временными рядами)

- **InfluxDB** – популярная база для сбора и анализа данных о временных рядах (например, IoT-устройства, логи).
- **TimescaleDB** – расширение PostgreSQL для работы с временными рядами.
- **OpenTSDB** – работает поверх HBase, предназначена для хранения больших объёмов временных данных.