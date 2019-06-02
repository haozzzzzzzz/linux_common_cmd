# Docker命令

```shell
# 进入docker容器
docker exec -it elasticsearch /bin/bash
docker run -h 指定内部访问域名，如docker run -h consul_server
docker run --link consul_server 在其他容器访问consul_server域名的服务
```

