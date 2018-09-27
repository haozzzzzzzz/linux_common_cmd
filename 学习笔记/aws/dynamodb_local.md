```shell
docker run -d -p 8000:8000 -v $PWD/data:/data/ --name dynamodb dwmkerr/dynamodb -dbPath /data/ -sharedDb
```

