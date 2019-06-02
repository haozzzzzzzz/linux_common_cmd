# docker kafka

##参考资料

- [在Docker环境下部署Kafka](http://blog.csdn.net/snowcity1231/article/details/54946857)



[TOC]

```shell
su # 进入root
docker ps # 查看启动的容器
docker images # 查看下载的镜像
docker run -d --name zookeeper -p 2181 -t wurstmeister/zookeeper # 启动zookeeper镜像
docker start zookeeper
docker start kafka
```



