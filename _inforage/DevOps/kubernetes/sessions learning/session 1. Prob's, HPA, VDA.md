- создать деплоймент
- kubectl apply -f deploy-echo.yaml
- kubectl get svc echo-service
- kubectl delete deployment echo-deployment
- kubectl delete -f deploy-echo.yaml
- kubectl delete svc echo-service
- kubectl delete ingress echo-ingress
- kubectl apply -f probes/deploy-echo-probes.yaml
- kubectl describe pod -l app=echo


Специально сломаем Liveness Probe

```yaml
livenessProbe:
  httpGet:
    path: /fail  # Несуществующий путь
    port: 5678
  periodSeconds: 5
```

- kubectl apply -f probes/service-echo-probes.yaml

## Создать докефайл, задеплоить в доке хаб
## Создать свое приложение и задеплоить