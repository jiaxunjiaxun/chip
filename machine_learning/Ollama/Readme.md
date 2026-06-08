# 记录安装Ollama的过程

## 系统条件

CPU：8核

内存：32G

显卡：Nvidia Tesla M60 / 双核16G

## 安装前准备

- 参考Rocky官方的安装方法
https://docs.rockylinux.org/desktop/display/installing_nvidia_gpu_drivers/
- 参考一些别的内容
  - https://www.cqmaple.com/202404/rocky-9-install-nvidia-driver.html
  - https://www.restack.io/p/ollama-answer-stable-diffusion-usage-cat-ai

```bash
## 管理员用户
su

## 环境准备
dnf update
dnf install epel-release
dnf groupinstall "Development Tools"
dnf install kernel-devel
dnf install dkms


## 添加N卡官方源
## 注意这个源是Rocky 9的，如果是其他版本要找到对应的源
dnf config-manager --add-repo http://developer.download.nvidia.com/compute/cuda/repos/rhel9/$(uname -i)/cuda-rhel9.repo


## 安装编译工具
dnf install kernel-headers-$(uname -r) kernel-devel-$(uname -r)
dnf install tar bzip2 make automake gcc gcc-c++ pciutils elfutils-libelf-devel libglvnd-opengl libglvnd-glx libglvnd-devel acpid pkgconf dkms


## 安装驱动
dnf module install nvidia-driver:latest-dkms


## 禁用grubby驱动，避免冲突
grubby --args="nouveau.modeset=0 rd.driver.blacklist=nouveau" --update-kernel=ALL


## 安装CUDA
dnf install cuda-12-...

## 构建并检测兼容性
dkms autoinstall -k $(uname -r)


## 重启生效
reboot now


## 查看显卡信息
nvidia-smi
```

## 安装Ollama

参考Ollama官方安装方法

https://github.com/ollama/ollama

```bash
# 管理员用户

curl -fsSL https://ollama.com/install.sh | sh
```

## 配置Ollama

```ini
# /etc/systemd/system/ollama.service

[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/usr/local/bin/ollama serve
User=ollama # 这地方不用改了
Group=ollama
Restart=always
RestartSec=3
Environment="PATH=/root/.local/bin:/root/bin:/home/deploy/.local/bin:/home/deploy/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin"

## 如果安装后就配置还比较好，
#WorkingDirectory=/opt/ollama
#Environment="OLLAMA_MODELS=/opt/ollama/models"
Environment="LD_LIBRARY_PATH=/usr/local/cuda/lib64"

## 显卡配置
Environment="OLLAMA_NUM_GPU=2"
Environment="OLLAMA_THREADS=2"
Environment="OLLAMA_MAX_THREADS=4"
Environment="OLLAMA_MAX_LOADED_MODELS=3"

## 开放访问
Environment="OLLAMA_HOST=0.0.0.0"

## API KEY
Environment="OLLAMA_API_KEY=1qaz@WSX3edc"

[Install]
WantedBy=default.target
```

## 测试接口

```bash
## Generate API
## 主要的聊天接口，一般只要配置服务的地址就好（http:\/\/localhost:11434）
curl http://localhost:11434/api/generate -d '{ "model": "deepseek-r1:7b", "prompt": "为什么草是绿的？" }' | jq

## Embedding API
## 提供向量化服务，可以配合postgre的向量数据库使用
curl http://localhost:11434/api/embed -d '{ "model": "bge-m3:latest", "input": "Why is the sky blue?" }' | jq
```