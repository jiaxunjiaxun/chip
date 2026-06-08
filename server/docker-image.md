# 如何制作Docker镜像（image）？

https://docs.docker.com/build/building/base-images/

https://docs.docker.com/get-started/09_image_best/

https://zhuanlan.zhihu.com/p/122380334

制作Docker镜像一般有2种方法：

1. 使用hub仓库中已有的环境，安装自己使用的软件环境后完成image创建
2. 通过Dockerfile，完成镜像image的创建

下面通过展示具体操作方法：

## 第一种：使用hub仓库中已有的环境，安装自己使用的软件环境后完成image创建。

制作自己的Docker镜像环境，里面包括：

1. centos
2. golang

```shell
# 1. pull 最新的 Centos 系统
docker pull centos

# 2. 运行进入容器
docker run -it centos /bin/bash

# 3. 在 Centos 环境中创建 work 用户
useradd work
su - work

# 4. 下载 Go 的 Linux 安装包，解压，配置环境变量
mkdir goapp && cd goapp && wget https://studygolang.com/dl/golang/go1.14.1.linux-amd64.tar.gz
tar zxvf go1.14.1.linux-amd64.tar.gz
vim ~/.bash_profile
source ~/.bash_profile
go version
echo $GOPATH
echo $HOME
# 此时，Go 的最基础环境就算配置好了，让我们写一个 Go 程序，运行一下吧~~~

# 5. Go实现"Hello World!"
vim /home/work/goapp/src/main.go
# package main
#
# import "fmt"
#
# func main() {
#     fmt.Println("Hello World");
# }

# 6. Go run main.go
go run /home/work/goapp/src/main.go

# 7. docker commit -m "[xxx]" -a "[authorName]" [containerID] [hub的名称]/[镜像名称]:[tag]

# 8. docker commit && push到远端仓
docker commit -m "centos and go env" -a "wenhan" 132aaafe685d zhangwenhan/gobox:v1
docker login

# 9. 查看 https://hub.docker.com/ 里的个人仓，push 的 image 已入库

# 10. 如果要打包报错到本地
docker save -o D:\DockerDesktop\vm-data\DockerDesktop\ebox\sunny_gobox.tar zhangwenhan/gobox:v1.0
```

## 第二种：通过Dockerfile，完成镜像image的创建。

1. 创建镜像所在文件夹 + Dockerfile 文件

```shell
mkdir ebox && cd ebox
touch Dockerfile
```
2. 在 Dockerfile 文件中写入指令

```shell
FROM ubuntu
RUN apt-get update && apt-get install -y ruby ruby-dev

# 格式说明：
# 每行命令都是以 INSTRUCTION statement 形式，就是命令+ 清单的模式。命令要大写，“#”是注解。
# FROM 命令是告诉docker 我们的镜像什么。
# RUN 命令是在镜像内部执行。就是说他后面的命令应该是针对镜像可以运行的命令。
```

3. 创建镜像

```shell
docker build -t zhangwenhan/ebox:v2.

# docker build 是 docker 创建镜像的命令
# -t 是标识新建的镜像属于 zhangwenhan 的
# ebox 是仓库的名称
# :v2 是 tag
# "."是用来指明 我们的使用的Dockerfile文件当前目录的
```

4. 创建完成后，从镜像创建容器

```shell
docker run -t -i zhangwenhan/ebox:v2 /bin/bash
```
