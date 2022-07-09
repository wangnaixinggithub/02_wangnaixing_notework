# Use-Gitee-Commit-Your-Code

# 1、New Repository

- 1、在码云中新建仓库，并`git clone` 到本地。

- 2、cd到该项目目录，把git clone 里面的东西全部复制下来。粘贴。

# 2、Need Config

3、设置全局git信息，你的用户名，你的邮箱。

```c#
$ git config --global user.name "你的名字或昵称"
$ git config --global user.email "你的邮箱"
```

# 3、Do Cmd

```shell
#将当前目录所有文件添加到git暂存区
git add . 
    
#提交并备注提交信息    
$ git commit -m "my first commit" 

#将本地提交推送到远程仓库
$ git push origin master 
```

