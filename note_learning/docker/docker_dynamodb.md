- https://hub.docker.com/r/dwmkerr/dynamodb
- https://hub.docker.com/r/amazon/dynamodb-local



```shell
docker run --name dynamodb -d -v $PWD/data:/data -p 8000:8000 dwmkerr/dynamodb -dbPath /data -sharedDb
```

