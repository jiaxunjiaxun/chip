# Ubuntu 安装 Nvidia GPU 驱动

## 禁用 Nouveau 驱动

```bash
# 1. 编辑黑名单配置
sudo vim /etc/modprobe.d/blacklist.conf

# 2. 末尾添加
# /etc/modprobe.d/blacklist.conf
blacklist nouveau
options nouveau modeset=0

# 3. 更新内核配置并重启
sudo update-initramfs -u
sudo reboot

# 4. 验证是否禁用成功（无输出则成功）
lsmod | grep nouveau
```

## 下载显卡驱动

[选择对应的驱动程度](https://www.nvidia.cn/drivers/lookup/)

```bash
sudo apt update
sudo apt install gcc make linux-headers-$(uname -r)

# [x] 默认无图形界面，无需操作。若使用带桌面的变体，需执行
sudo systemctl stop gdm3

sudo chmod +x NVIDIA-Linux-x86_64-550.xx.run

# --no-opengl-files 禁用 OpenGL 文件（避免图形界面冲突）
# --no-x-check 忽略 X 服务检查
# --dkms 启用 DKMS 支持（内核更新后自动重编译）
sudo ./NVIDIA-Linux-x86_64-550.xx.run --no-opengl-files --no-x-check --dkms
```

## 验证多显卡

```bash
# 验证多卡识别
nvidia-smi -L

# 启用持久模式（避免 GPU 休眠）
sudo nvidia-smi -pm 1

# --- 检查 PCIe 通道状态

# [x] 配置多 GPU 通信（如 NVLink）
sudo apt install nvidia-nvswitch

# 查看双卡 PCIe 连接状态
lspci -vv | grep -i nvidia

# 检查电源状态
nvidia-smi -q | grep -i "power draw"

# --- 修复常见双卡问题

# 启用持久化模式 (双卡必需)
sudo nvidia-smi -pm 1

# 禁用显卡自动降频
sudo nvidia-smi -lgc 0,0

# 创建双卡设备文件
sudo nvidia-modprobe -c 0 -u
sudo nvidia-modprobe -c 1 -u

# --- 验证修复

# 检查驱动状态
cat /proc/driver/nvidia/version

# 验证双卡识别
nvidia-smi -L

# 完整系统诊断
nvidia-bug-report.sh
```

## 检查问题

```bash
# 1. 检查内核模块状态

# 检查 NVIDIA 内核模块是否加载
lsmod | grep nvidia

# 若无输出，手动加载模块
sudo modprobe nvidia

# 检查加载状态
dmesg | grep nvidia

# 2. 重建驱动内核模块 (DKMS)

# 重新安装内核头文件
sudo apt install --reinstall linux-headers-$(uname -r)

# 重建 DKMS 模块
sudo dkms install -m nvidia -v $(modinfo -F version nvidia)

# 3. 禁用 Secure Boot

# 检查 Secure Boot 状态
mokutil --sb-state

# 若显示 "SecureBoot enabled"，需在 BIOS 中禁用它
# 重启进入 BIOS > Security > Secure Boot > Disable

# 4. 检查驱动冲突

# 查看当前加载的显卡驱动
lspci -k | grep -EA3 'VGA|3D|Display'

# 完全移除冲突模块
sudo rmmod nouveau
sudo rmmod nvidia_drm
sudo rmmod nvidia_modeset
sudo rmmod nvidia_uvm
sudo rmmod nvidia

# 5. 重新安装驱动

# 进入无界面模式 (重要!)
sudo systemctl isolate multi-user.target

# 卸载现有驱动
sudo nvidia-uninstall

# 重新安装驱动 (使用您下载的版本号)
sudo sh NVIDIA-Linux-x86_64-550.xx.run --no-opengl-files --no-x-check --dkms --silent

# 6. 更新初始化内存盘

# 强制更新 initramfs
sudo update-initramfs -u -k all

# 重启系统
sudo reboot
```

---

https://blog.csdn.net/wm9028/article/details/110268030

要在 Ubuntu 24.04 中卸载显卡驱动，可以按照以下步骤操作：
禁用显卡驱动：打开终端，输入以下命令禁用Nvidia驱动：
sudo systemctl stop nvidia-persistenced
卸载Nvidia驱动：在终端中输入以下命令卸载Nvidia驱动：
sudo apt-get remove --purge nvidia-*
删除配置文件：运行以下命令删除与驱动相关的配置文件：
sudo rm /etc/X11/xorg.conf
清理无用包：卸载完成后，运行以下命令清理系统中无用的包：
sudo apt-get autoremove
更新initramfs：最后，更新initramfs以确保系统正常运行：
sudo update-initramfs -u