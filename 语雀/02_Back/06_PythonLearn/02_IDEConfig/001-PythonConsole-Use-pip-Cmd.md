# Use-pip-Cmd

- 在Python开发的时候，我们需要pip命令下载一些依赖。

# 1、Need Config

- 让pycharm控制台 可 使用 pip模块命令。

![image-20220529113758990](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220529113758990.png)

- 以管理员身份进入powerShell,设置执行策略为`RemoteSigned`

```shell
Windows PowerShell
版权所有（C） Microsoft Corporation。保留所有权利。

安装最新的 PowerShell，了解新功能和改进！https://aka.ms/PSWindows

PS C:\WINDOWS\system32> set-ExecutionPolicy RemoteSigned

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): Y
PS C:\WINDOWS\system32>
```

# 2、Can Use 

- 比如我下载Python方法Redis的依赖库。

```
pip install redis  -i http://pypi.doubanio.com/simple/ --trusted-host pypi.doubanio.com
```

![image-20220529114440797](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220529114440797.png)