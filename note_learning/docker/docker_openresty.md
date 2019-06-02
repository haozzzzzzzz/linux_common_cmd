# Docker OpenResty

- https://hub.docker.com/r/openresty/openresty

```
docker run --name openresty -p 80:80 -v /Users/hao/Documents/Store/openresty/nginx_config:/usr/local/openresty/nginx/conf/conf.d -v /Users/hao/Documents/Store/openresty/project:/usr/local/openresty/project -d haozzzzzzzz/openresty:master
```

