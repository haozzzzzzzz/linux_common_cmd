# CentOS7安装MariaDB 10.1

CentOS7默认安装的MariaDB的版本是5.5.x，旧版MariaDB有很多BUG，升级到最新版本时，需要添加新版本的repo，然后再安装。

## 添加MariaDB 10.1 YUM repo文件

```shell
cat <<EOF | sudo tee -a /et/yum.repos.d/MariaDB.repo
# MariaDB 10.1 CentOS repository list
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.1/centos7-amd64
gpgkey = gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck = 1
EOF
```



## 使用YUM安装MariaDB

```shell
sudo yum install MariaDB-server MariaDB-client -y
```



## 启动和加入到开机启动

```shell
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service
```



## 加固MariaDB的安装

```shell
sudo /usr/bin/mysql_secure_installation
```

Answer questions as below, and ensure that you will use your own MariaDB root password:

- Enter current password for root (enter for none): Just press the `Enter` button
- Set root password? [Y/n]: `Y`
- New password: `your-MariaDB-root-password`
- Re-enter new password: `your-MariaDB-root-password`
- Remove anonymous users? [Y/n]: `Y`
- Disallow root login remotely? [Y/n]: `Y`
- Remove test database and access to it? [Y/n]: `Y`
- Reload privilege tables now? [Y/n]: `Y`