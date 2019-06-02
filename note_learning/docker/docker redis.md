# docker redis

参考

- [https://hub.docker.com/_/redis/](https://hub.docker.com/_/redis/)

```shel
docker run --name redis -p 6379:6379 -v "$PWD/data":/data -d redis redis-server --appendonly yes
```

