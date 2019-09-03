# git命令

##分支

```shell
# 创建分支
git branch <branch_name>

# 根据分支建立分支
git branch <new_branch_name> <exists_branch_name>

# 进入分支
git checkout <branch_name>
```



```sh
# 创建并进入分支
git checkout -b <branch_name>
```



```shell
# 合并分支
git merge <branch_name>
```



```sh
# 删除分支
git branch -d <branch_name>
```



根据tag建branch

```
git branch <branch_name> <tag_name>
```





## 标签

```shell
# 列出已有标签
git tag

# 过滤
git tag -l "v1.4.2.*"

# 标签有轻量级标签和含附注的标签，常用轻量级标签
git tag v1.7-2018-06-15-1
git show v1.7-2018-06-15-1

# 删除
git tag -d v1.7-2018-06-15-1

# 推送
git push origin v1.7-2018-06-15-1
# 推送所有tags
git push origin --tags
git push --tags
```



## 恢复

```shell
# 特定文件
git checkout -- <文件名>

# 当前所有更改
git checkout --
```



## 解决冲突

```
git checkout <文件名> # 使用远程文件
```



## 版本回滚

```sh

```



## gitconfig

```
[url "git@github.com:"]
    insteadOf = https://github.com/
    
将git中所有的http请求替换成ssh的请求
```



