# Docker中安装Consul

- https://www.consul.io/docs/guides/consul-containers.html

```sh
docker pull consul

# 启动server
docker run -d --name consul_server -h consul_server -p 127.0.0.1:8500:8500 -v "$PWD/server_data":/consul/data -e CONSUL_BIND_INTERFACE=eth0 -e CONSUL_CLIENT_INTERFACE=eth0 consul agent -server -ui

# 启动client
docker run -d -e CONSUL_BIND_INTERFACE=eth0 -e CONSUL_CLIENT_INTERFACE=eth0 -v "$PWD/client_data":/consul/data --name consul_client --link consul_server consul agent -dev -join=consul_server
```

