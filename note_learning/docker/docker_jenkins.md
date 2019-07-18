# Docker Jenkins

https://github.com/jenkinsci/docker/blob/master/README.md



with docker

```sh
docker run --name jenkins -d -v $PWD/jenkins_home:/var/jenkins_home -v $PWD/data:/data -v /var/run/docker.sock:/var/run/docker.sock -p 8082:8080 -p 50000:50000 haozzzzzzzz/jenkins_docker
```

