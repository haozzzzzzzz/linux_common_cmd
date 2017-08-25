# ssh

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