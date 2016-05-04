# Aliyun

## mount data disk

    fdisk -l
    fdisk /dev/xvdb[aliyun mount path]
    mkfs.ext3 /dev/xvdb1
    mkdir /path/to/mount_point
    mount /dev/xvdb1 /path/to/mount_point
    
    echo '/dev/xvdb1 /mnt ext3 defaults 0 0' >> /etc/fstab
    mount -a