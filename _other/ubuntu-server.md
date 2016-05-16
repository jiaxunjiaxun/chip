# Configure

## Console

### Language

Fix file: /etc/default/locale

    LANG="en_US.UTF-8"
    LANGUAGE="en_US:en"

    if [ -z "$DISPLAY" ]; then
        export LANG=en_US.UTF-8
        unset LANGUAGE
    fi

### Screen

- 打开grub文件（vim /etc/default/grub），修改参数GRUB_CMDLINE_LINUX的值，GRUB_CMDLINE_LINUX="vga=0x317"
<table><caption>参数值参考</caption><thead><tr><th>#</th><th>640x480</th><th>800x600</th><th>1024x768</th><th>1280x1024</th></tr></thead><tbody><tr><th>256</th><td>0x301</td><td>0x303</td><td>0x305</td><td>0x307</td></tr><tr><th>32K</th><td>0x310</td><td>0x313</td><td>0x316</td><td>0x319</td></tr><tr><th>64K</th><td>0x311</td><td>0x314</td><td>0x317</td><td>0x31A</td></tr><tr><th>16M</th><td>0x312</td><td>0x315</td><td>0x318</td><td>0x31B</td></tr></tbody></table>
- update-grub
- reboot

### Shutdown

1. shutdown –help 可以查看shutdown命令如何使用，当然也可以使用man shutdown命令。
2. shutdown -h now 现在立即关机
3. shutdown -r now 现在立即重启
4. shutdown -r +3 三分钟后重启
5. shutdown -h +3 “The System will shutdown after 3 minutes” 提示使用者将在三分钟后关机
6. shutdown -r 20:23 在20：23时将重启计算机
7. shutdown -r 20:23 & 可以将在20：23时重启的任务放到后台去，用户可以继续操作终端