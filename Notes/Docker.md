# Docker Notes

[toc]

## install and config
### INSTALL AND PULL
1. install docker desktop
2. pull image of centos
```shell
    docker search centos
    docker pull centos
```
3. time out
```json
    "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://gah7qgxv.mirror.aliyuncs.com"
  ]
```
4. docker images
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              831691599b88        7 weeks ago         215MB
```
5. docker run -it centos 这个时候应该已经进入linux命令行了，输入ls确认下
6. 另外起一个cmd docker container ls
```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
fa0392d26607        centos:latest       "/bin/bash"         17 seconds ago      Created                                 x_name
```
7. docker rename fa0392d26607 junyi_cpp
8. docker ps 看到自己名字的容器在运行 就ok了
9. docker stop junyi_cpp
10. 以上步骤只是确定docker运行正常，创建的这些都可以删掉
### CONFIG
1. FILE SHARING
小海豚settings里设置共享文件夹 通过-v 来挂载
2. 端口映射：建议-P 随机映射
3. image是静态的，把他run起来作为container使用,所以把端口，文件共享，交互后台运行都算上，命令如下
```shell
docker run -p 8080:8080 -p 8888:8888 -p 7193:7193 -p 8000:8000 -p 9999:9999 -dit -v D:/DockerShare:/home centos

docker rename 8a3b1983159a junyi_cpp 这个id需要自己docker ps看
```
4. 进入容器
```shell
docker exec -it 8a3b1983159a /bin/bash
```
这样就进到了linux环境下
5. 退出容器 exit 退出后 docker ps看一下 容器应当还在运行中
### daily work
```shell
docker start junyi_cpp
docker exec -it junyi_cpp /bin/bash
一顿操作
exit
```

