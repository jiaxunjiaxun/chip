# Docker 常用命令

[ref](https://docs.docker.com/)\
[ref](https://zhuanlan.zhihu.com/p/142195798)

## Docker环境信息

``` bash
# 用于检测 Docker 是否正确安装，一般结合 docker [ info | version ] 命令一起使用
docker info
docker version
```

## Docker运维操作

``` bash
# docker attach 命令对应用开发者很有用，可以连接到正在部署的容器，观察容器的运行状况，或与容器的主进程进行交互。
docker attach

# 用于查看镜像和容器的详细信息，默认会列出全部信息，可以通过--format 参数来指定输出的模板格式，以便输出特定格式。
docker inspect

# docker ps 命令可以查看容器的 CONTAINER ID、NAME、IMAGE NAME、端口开启及绑定容器启动执行的 COMMAND。最常用的功能是通过 ps 来找到CONTAINER ID，以便对特定容器进行操作。
docker ps
docker ps # 默认显示当前正在运行中的container
docker ps -a # 查看包括已经停止的所有容器
docker ps -l # 显示最新启动的一个容器（包括已停止的）

# 列出机器上镜像。其中我们可以根据 REPOSITORY 来判断这个镜像是来自哪个服务器，如果没有 / 则表示官方镜像，类似于 username/repos_name 表示GitBub 的个人公共库，类似于 http://regsistory.example.com:5000/repos_name 则表示的是私服。
docker images

# 在 docker index 中搜索 image，搜索的范围是官方镜像和所有个人公共镜像。NAME列的 / 后面是仓库的名字。
# docker search [name]
docker search nginx

# 从 docker registry server 中下 pull image 或 repository
# docker pull [images-name]
docker pull redis # 上面的命令需要，在docker v1.2版本之前，会下载官方镜像的centos仓库的所有镜像，而从v1.3开始。官方文档的说明变了，will pull the centos:latest image, its intermediate layers and any aliases of the same id，也就是只会下载tag为latest的镜像（以及同一images id 的其他tag）
docker pull centos:centos # 明确指定具体的镜像
docker pull seanlook/centos:centos6 # 从某个的公共仓库拉取，形如 docker pull username/repositroy<:tag_name>
docker pull dl.dockerpool.com:5000/mongo:latest # 如果你没有网络，或者从其它私服获取镜像，形如docker pull http://registry.domain.com:5000/mongo:latest

# 推送一个 images 或 repository 到 registry（push）
# docker push [images-name]
# 与上面的 pull 对应，可以推送到 Docker Hub 的 Public、Private 以及私服、但不能推送到 Top Level Repository
# 在 repostiroy 不存在的情况下，命令行下 push 上去的会为我们创建私有库，然而通过浏览器创建的默认为公共库。
docker push seanlook/mongo
docker push registry.tp-link.net:5000/mongo

# 从 images 启动一个 container
# 实例安装并运行 rabbitMQ
docker run -d --hostname my-rabbit -p 5672:5672 -p 15672:15672 rabbitmq:3.73-management
```

当利用docker run 来创建容器时，docker在后台运行的标准包括：

- 检查本地是否存在指定镜像，不存在就从公有仓库下载。
- 利用镜像创建并启动一个容器
- 分配一个文件系统，并在只读的镜像层外面挂载一层可读写层。
- 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去。
- 从地址池配置一个ip地址给容器
- 执行完毕后容器被终止。

``` bash
# 使用 image 创建 container 并执行相应命令，然后停止。
docker run ubuntu echo 'hello world'
```

这是简单的方式，跟在本地直接执行 echo 'hello world' 几乎感觉不到任何区别，而实际上它会从本地ubuntu：latest镜像启动到一个容器，并执行打印命令退出（docker ps -l 可查看）.需要注意的是，默认有一个rm=true的参数，即完成操作后停止容器并从文件系统移除。因为docker的容器实在太轻量级了，很多时候用户都是随时删除和新创建容器。

容器启动后会自动随机生成一个Container ID，这个 ID 在后面 commit 命令后可以变为 IMAGE ID。

``` bash
# 使用 image 创建 container 并进入交互模式，login shell 是 /bin/bash
docker run -i-t --name myubuntu ubuntu /bin/bash
```

上面的--name参数可以指定启动后的容器名字，如果不指定则docker会帮我们取一个名字。镜像ubuntu可以用IMAGE ID (dba1062371c)代替，并且会启动一个伪终端，但通过ps或top命令我们却只能看到一两个进程。因为容器的核心是所执行的应用程序，所需要的资源都是应用程序运行所必须的，除此之外，并没有其它的资源，可见Docker对资源的利用率极高，此时使用exit或ctrl+d退出后，这个容器也就消失了 。

``` bash
# 运行出一个container放到后台运行
docker run -d hello-world
```

它将直接把启动的container挂起放在后台运行（这才叫saas），并且会输出一个CONTAINER ID，通过docker ps可以看到这个容器的信息，可在container外面查看它的输出docker logs ae60c4b64205，也可以通过docker attach ae60c4b64205连接到这个正在运行的终端，此时在Ctrl+C退出container就消失了，按ctrl-p ctrl-q可以退出到宿主机，而保持container仍然在运行

另外，如果-d启动但后面的命令执行完就结束了，如/bin/bash、echo test，则container做完该做的时候依然会终止。而且-d不能与--rm同时使用

可以通过这种方式来运行memcached、apache等。

``` bash
# 映射host到container的端口和目录
# -p 80:80 这个即是默认情况下，绑定主机所有网卡（0.0.0.0）的80端口到容器的80端口上。
# -p 127.0.0.1:80:80 只绑定localhost这个接口的80的接口
```

``` bash
# 将一个container固化成一个新的iamge(commit)
# 当我们在制作自己的镜像的时候，会在container中安装一些工具、修改配置，如果不做commit保存起来，那么container停止以后再启动，这些更改就消失了。
docker commit <container> [repo:tag]
```

后面的repo:tag可选

只能提交正在运行的 container，即通过 docker ps 可以看见的容器

``` bash
# 启动一个已存在的容器（run 是从image新建容器后再启动），以下也可以使用docker start 容器id
docker start [image id]
```

## 常用的基本操作

### 开启/停止/重启container（start/stop/restart）

容器可以通过run 新建一个来运行，也可以重建start已经停止的container，但start不能够再指定容器启动时运行的指令，因为docker只能有一个前台进程。

容器stop （或 ctrl+d）时，会在保存当前容器的状态之后退出，下次start时保存上次关闭时更改。而且每次进入attach进去的界面是一样的，run启动或commit提交的时刻相同。

### 连接到正在运行中container（attach）

attach上去的容器必须正在运行，可以同时连接上同一个container来共享屏幕（与screen命令的attach类似）。

官方文档中说attach后可以通过CTRL-C来detach，但实际上经过我的测试，如果container当前在运行bash，CTRL-C自然是当前行的输入，没有退出；如果container当前正在前台运行进程，如输出nginx的access.log日志，CTRL-C不仅会导致退出容器，而且还stop了。这不是我们想要的，detach的意思按理应该是脱离容器终端，但容器依然运行。好在attach是可以带上--sig-proxy=false来确保CTRL-D或CTRL-C不会关闭容器。

### 查看image或container的底层信息（inspect）

inspect的对象可以是image、运行中的container和停止的container。

### 删除一个或多个container,image(rm,rmi)

你可能在使用过程中会build或commit许多镜像，无用的镜像需要删除。但删除这些镜像是有一些条件的：

- 同一个IMAGE ID可能会有多个TAG（可能还在不同的仓库），首先你要根据这些 image names 来删除标签，当删除最后一个tag的时候就会自动删除镜像；
- 承上，如果要删除的多个IMAGE NAME在同一个REPOSITORY，可以通过docker rmi 来同时删除剩下的TAG；若在不同Repo则还是需要手动逐个删除TAG；
- 还存在由这个镜像启动的container时（即便已经停止），也无法删除镜像；

``` bash
docker rm <container_id/contaner_name> # 删除容器
docker rm $(docker ps -a -q) # 删除所有停止的容器
docker rmi <image_id/image_name ...> # 删除镜像
```

### 查看容器的信息container（ps）

``` bash
# docker ps命令可以查看容器的CONTAINER ID、NAME、IMAGE NAME、端口开启及绑定、容器启动后执行的COMMNAD。经常通过ps来找到CONTAINER_ID。 docker ps 默认显示当前正在运行中的container docker ps -a 查看包括已经停止的所有容器
# docker ps -l 显示最新启动的一个容器（包括已停止的）
docker ps -a
docker ps -l
```

### 查看容器中正在进行的进程

容器运行时不一定有/bin/bash终端来交互执行top命令，查看container中正在运行的进程，况且还不一定有top命令，这是docker top 就很有用了。实际上在host上使用ps -ef|grep docker也可以看到一组类似的进程信息，把container里的进程看成是host上启动docker的子进程就对了。