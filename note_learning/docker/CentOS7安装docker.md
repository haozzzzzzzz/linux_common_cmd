# CentOS7安装Docker

## CentOS7虚拟机安装

安装版本为Minimal Install



## docker软件包安装

- https://docs.docker.com/install/linux/centos/



移除旧版本docker

```shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

安装Docker CE

### 从源中安装

1. 安装依赖包。

   ```shell
   yum install -y yum-utils \
     device-mapper-persistent-data \
     lvm2
   ```

   ​

2. 添加源。国内无法访问此链接，需要添加代理才可以。

   ```shell
   yum-config-manager \
       --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo
   ```

   ​

3. （可选）启用`edge`和`test`仓库。如果要关闭，可以使用`—disable`选项。

   ```
   yum-config-manager --enable docker-ce-edge
   yum-config-manager --enable docker-ce-test
   ```

   ​

4. 安装 Docker CE

   ```shell
   yum install docker-ce
   ```

5. 运行 Docker

   ```shell
   systemctl start docker
   ```

   ​

6. 运行hello-world镜像

   ```
   docker run hello-world
   ```



## 使用国内镜像仓库

- https://www.docker-cn.com/registry-mirror

新建文件`/etc/docker/daemon.json`,添加配置。

```Son
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

然后重启docker

```shell
systemctl restart docker
```



也可以在安装的时候使用阿里云的镜像进行加速。



## 下载centos镜像，开始docker之旅

```shell
docker search centos
docker pull centos
docker run centos /bin/echo "Hello, world." # 启动centos并执行输出命令，最后退出
```

对docker容器来说，当运行的应用退出后，容器会自动退出。