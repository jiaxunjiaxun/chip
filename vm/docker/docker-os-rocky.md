## Install on Rocky Linux 9

https://docs.rockylinux.org/gemstones/containers/docker/
https://www.rockylinux.cn/notes/zai-rocky-linux-9-1-shang-an-zhuang-docker-ce.html

```shell
dnf config-manager --add-repo=https://download.docker.com/linux/rhel/docker-ce.repo

dnf update

dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin

systemctl --now enable docker
```