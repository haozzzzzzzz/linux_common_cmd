# ssh



## ssh服务器

### ssh使用证书登陆

1. 使用用户账户登陆系统

2. 生成公钥和私钥。默认生成到`~/.ssh/`下,`id_rsa`为私钥，`id_rsa.pub`为公钥。公钥放到要登陆的服务器，在ssh客户端配置私钥。

   ```shell
   ssh-keygen -t rsa
   ```

3. 将公钥追加给服务器的授权key。

   ```shell
   cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
   ```

4. 更改文件权限

   ```shell
   chmod -R 0700 ~/.ssh/
   chmod 0644 ~/.ssh/authorized_keys
   ```

5. 编辑sshd配置文件`/etc/ssh/sshd_config`，设置以下选项

   ```shell
   StrictModes no
   PubkeyAuthentication yes
   ```

   线上可关闭密码登陆

   ```shell
   PasswordAuthentication no
   ```

6. 重启sshd即可生效。

   ```shell
   systemctl restart sshd
   ```

7. 在客户端（如xshell）配置域名和证书即可登入。

8. 将相同的pub文件部署到其他要部署的服务器可以实现，相同key登入多个服务器。如果没有`.ssh`文件夹可手动创建。



##ssh客户端

ssh-add失败

```shell
[root@localhost keys]# ssh-add ./luoh
Could not open a connection to your authentication agent.
```



原因，未启动ssh-agent.

```shell
exec ssh-agent bash
ssh-add ./luoh
```

如果提示权限太开放，则改为600。

客户端远程执行多条命令。`<< eeooff `脚本内容`eeooff`

```shell
ssh dev@$1 -tt -i ${KEY} << eeooff
cd /home/user/dev/softs/guard
killall -9 guard
nohup ./guard -c=./guard.toml> guard.log 2>&1 &
exit
eeooff

```



