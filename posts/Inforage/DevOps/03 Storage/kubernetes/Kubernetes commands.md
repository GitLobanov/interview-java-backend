# Get

```bash
k get svc
k get deploy
k get pods
```

# Create

```bash
k create deploy k8s-deploy-hello --image=lobanovdocker/k8s-web-hello:1.0.0
```

# Delete

```bash
k delete svc k8s-deploy-hello
k delete deploy k8s-deploy-hello
```
# Set

```bash
k set image deploy k8s-deploy-hello k8s-web-hello=lobanovdocker/k8s-web-hello:2.0.0
```

# Rollout

```bash
k rollout status deploy k8s-deploy-hello
```
# Describe

```bash
k describe deploy k8s-deploy-hello
```

# Scale

```bash
k scale deploy k8s-deploy-hello --replicas=20
```

# Apply

```bash
k appply -f deployment.yml
```

