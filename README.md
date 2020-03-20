# docker-haproxy-logging

HAproxy with logging Dockerfile.

## Usage

Testing.

```bash
docker-compose build
docker-compose up -d
curl http://localhost
curl http://localhost
curl http://localhost
...
```

```bash
tail -f share/haproxy/log/haproxy.log
```
