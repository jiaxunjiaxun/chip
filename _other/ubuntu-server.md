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