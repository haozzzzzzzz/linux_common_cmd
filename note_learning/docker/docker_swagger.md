```
# swagger编辑器
docker pull swaggerapi/swagger-editor
docker run --name swaggered -d -p 8081:8080 swaggerapi/swagger-editor

# swagger ui
docker pull swaggerapi/swagger-ui
docker run --name swaggerui -d -p 8082:8080 swaggerapi/swagger-ui
```

http://127.0.0.1:8081



- go-swagger安装https://goswagger.io/install.html

```
docker pull quay.io/goswagger/swagger

alias swagger="docker run --rm -it -e GOPATH=$HOME/go:/go -v $HOME:$HOME -w $(pwd) quay.io/goswagger/swagger"
swagger version
```



