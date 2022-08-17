在powershell中执行命令

# 1、容器的使用

```bash
//查看docker命令
docker
//深入了解命令的使用
docker command --help
//获取镜像
docker pull ubuntu
//启动容器
docker run [参数] [容器名] [指令] 
例：docker run -it ubuntu /bin/bash 
	-i	交互式操作
	-t	终端
	ubuntu	镜像
	/bin/bash	交互式命令
	exit	退出终端
//查看所有的容器
docker ps -a
//启动一个已经停止的容器
docker start [容器id]
//
```

