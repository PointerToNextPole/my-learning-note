# Docker踩坑与解决方案

键入docker命令时（比如：`docker ps`）出现如下提示：

```
error during connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.39/containers/json: open //./pipe/docker_engine: The system cannot find the file specified. In the default daemon configuration on Windows, the docker client must be run elevated to connect. This error may also indicate that the docker daemon is not running.
```

**解决方案：**

- 进入docker安装地址 `C:\Program Files\Docker\Docker`（默认安装地址为`C:\Program Files\Docker\Docker`）
- 键入`./DockerCli.exe -SwitchDaemon`

就正常了。

参考：[[docker][win10]安装的坑](https://www.jianshu.com/p/09d53c822cf8)



## 关于Docker for Windows docker pull的镜像存储位置

参见如下两篇：

[思否 - Docker for Windows docker pull 镜像到哪里去了](https://segmentfault.com/q/1010000006745913)

[docker for windows pull镜像文件的安装位置改变方法](https://blog.csdn.net/StemQ/article/details/53150939)