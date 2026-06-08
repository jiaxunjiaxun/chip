# Docker 常用命令

## 安装

```shell
$(. /etc/os-release && echo "$VERSION_CODENAME")

dpkg --print-architecture
```

- [docker-debian.md](docker-debian.md)
- [docker-rocky.md](docker-rocky.md)
- [docker-ubuntu.md](docker-ubuntu.md)

## 命令

Remove and upgrade docker containers and images

```shell
sudo docker stop $(sudo docker ps -aq)
sudo docker rm $(sudo docker ps -aq)
sudo docker rmi $(sudo docker images -q)

sudo docker system prune -f
sudo docker container prune -f
sudo docker image prune -af
```

[note] Switch to the mirror of Tsinghua University

```shell
sed -i 's+https://download.docker.com+https://mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```

[note] docker hub

```shell
sudo vim /etc/docker/daemon.json

{
    "registry-mirrors": [
        "https://docker.1ms.run",
        "https://docker.xuanyuan.me"
    ]
}

sudo systemctl daemon-reload
sudo systemctl restart docker

# 允许非 root 用户运行 Docker
sudo usermod -aG docker $USER
```

[note] docker gpu

```shell
# 添加密钥和仓库
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit

# 生成配置文件
sudo nvidia-ctk runtime configure --runtime=docker

# 重启Docker服务
sudo systemctl restart docker
```
