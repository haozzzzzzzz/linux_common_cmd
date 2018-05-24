# docker mysql

参考

- [https://hub.docker.com/_/mysql/](https://hub.docker.com/_/mysql/)

```shell
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -e /Users/hao/Documents/Store/mysql/data:/var/lib/mysql -d mysql:latest
```



处理连接问题

https://blog.csdn.net/qq_24664619/article/details/80263546