# Найти 

- поиск

```bash
find / -name project | grep project
ls -l
```

- поиск дериктории по порту процесса

```bash
lsof -p 8080
lsof -i :5000

```

# Процессы

```bash
ps ax
ps aux
htop
kill <PID>
```

Запустить на фоне

```bash
nohup npm start &
cat hohup.out (logs)
```

# Команды для сетевой диагностики

```bash
apt install net-tools 
netstat -tuln
```