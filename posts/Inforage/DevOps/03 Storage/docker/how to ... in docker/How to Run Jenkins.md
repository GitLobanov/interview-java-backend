# CLI

```bash
cat home/user/.ssh/jenkins_key.pub
```

```bash
curl -Lv http://localhost:8085/login 2>&1 | grep -i 'x-ssh-endpoint'
```

```bash
ssh -i /home/mike/.ssh/jenkins_key -l mike -p 8022 jenkins-server help
```

Keep a note of the options used with the SSH command:  

1. `-i` flag is used to point to `mike's` private SSH key. Remember, we have already added the public key in the Jenkins configuration.  
2. `-l` is the login user which in our example is `mike`  
3. `-p` is the port which we found out in the previous step to be `8022`

Try running the below command to restart jenkins from the CLI:

```bash
ssh -i /home/mike/.ssh/jenkins_key -l mike -p 8022 jenkins-server safe-restart
```
# Pipeline 

Simple echo hello world

```
pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```

# Docker-compose

```c
version: '3.8'

services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins-server
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
    networks:
      - jenkins-network

  jenkins-agent:
    image: jenkins/inbound-agent
    container_name: jenkins-agent
    environment:
      - JENKINS_URL=http://jenkins-server:8080
      - JENKINS_AGENT_NAME=agent
      - JENKINS_AGENT_WORKDIR=/home/jenkins/agent
      - JENKINS_SECRET=<your-secret-here> # Укажите секрет здесь
    volumes:
      - agent_workdir:/home/jenkins/agent
    depends_on:
      - jenkins
    networks:
      - jenkins-network

volumes:
  jenkins_home:
  agent_workdir:

networks:
  jenkins-network:
```
# Resources

- Помогает поднять дженкинс сервер и одну ноду [Как автоматизировать настройку Jenkins с помощью Docker](https://timeweb.cloud/tutorials/ci-cd/avtomatizaciya-nastrojki-jenkins-s-pomoshchyu-docker)
- Настройки связи дженкинса с гит хабом [Как настроить непрерывную интеграцию в Jenkins при отправке изменений в репозиторий](https://habr.com/ru/companies/slurm/articles/721520/)
- [Развёртывание API Spring Boot 3.0 с использованием Jenkins Pipeline и Docker](https://uproger.com/razvertyvanie-api-spring-boot-s-ispolzovaniem-jenkins/)
- [2017 Configuring Jenkins with Github](https://medium.com/dev-blogs/configuring-jenkins-with-github-eef13a5cc9e9)