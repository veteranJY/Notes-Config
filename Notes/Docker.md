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
小海豚settings里设置共享文件夹

