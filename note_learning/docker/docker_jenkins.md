# Docker Jenkins

https://github.com/jenkinsci/docker/blob/master/README.md



```sh
docker run --name jenkins -d -v $PWD/jenkins_home:/var/jenkins_home -p 8082:8080 -p 50000:50000 jenkins/jenkins
```

