### Docker

```bash
docker cp db.backup a-tink-backend-postgres-1:/tmp/db.backup
docker exec -it a-tink-backend-postgres-1 psql -U local -d dvdrental -f /tmp/db.backup
```