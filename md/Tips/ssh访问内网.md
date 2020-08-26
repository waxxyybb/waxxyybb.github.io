# ssh访问内网

## 1  需求
很简单的一个需求，我在寝室想要访问自己实验室的机器  
虽然都在校园网内但隶属于不同的子网，Teamviewer效率低下  
通信场景如下，我想通过任意计算机C访问内网中的计算机A  
在机器A不具备公网IP的情况下，借助公网机器B，完成ssh内容的转发  
![通信场景](assets/images/tips/01.png)

## 2  准备
机器A  
用户名：user_name_A  
IP：192.168.1.252（内网IP）

机器B  
用户名:  user_name_B  
IP：x.x.x.x（公网IP）  

机器C  
任意计算机，想要访问机器A

## 3  步骤
### 3.1  端口映射
在机器A上执行  
` ssh -NfR 20000:localhost:22 user_name_B@x.x.x.x -o ServerAliveInterval=300 `
- 关于-NfR详细参见`man ssh`
- ServerAliveInterval的目的是为了防止ssh链接长时间不通信而自动断开

**如何验证上述命令是否成功？**  
在机器B上执行  
`ss -nat|grep 20000`或者`netstat -a|grep 20000`  
查看端口20000已经处于监听状态后，继续在机器B上执行  
`ssh user_name_A@localhost -p20000`  
输入完user_name_A的密码后，如果成功ssh进机器A，说明绑定成功

### 3.2  配置ssh文件
虽然此时公网机器B已经可以穿透内网访问A了，但我们需要的是通过机器C访问机器A，因此还需要额外的配置  
在机器B上执行  
修改 `/etc/ssh/sshd_config`文件，设置  
`GatewayPorts yes`  
而后重启ssh服务  
`systemctl restart ssh.service `

### 3.3  测试功能
在机器C上执行  
`ssh user_name_A@x.x.x.x -p20000`  
成功穿透内网  

## 4  尾语
这应该是目前为止，最简单的解决方案了
- 关于开机启动项的内容不在此赘述
- 依样画葫芦，通过ssh转发其他端口，内网的FTP、web、jupyter notebook也可以挂载到公网上

话说，啥时候ipv6才能真正普及呢～～



## 参考对象
[https://blog.mythsman.com/2017/01/14/1/](https://blog.mythsman.com/2017/01/14/1/)  
[https://blog.csdn.net/quantumenergy/article/details/78244003](https://blog.csdn.net/quantumenergy/article/details/78244003)  
[http://whatbeg.com/2018/12/05/jupyternotebook-1.html](http://whatbeg.com/2018/12/05/jupyternotebook-1.html)
