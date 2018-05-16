# docker mysql

参考

- [https://hub.docker.com/_/mysql/](https://hub.docker.com/_/mysql/)

```shell
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -e "$PWD/data":/var/lib/mysql -d mysql:latest
```

