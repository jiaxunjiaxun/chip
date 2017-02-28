# Firewall Service

[Firewall](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7)
[Firewall](http://www.tuicool.com/articles/vMr6Vj)
[Firewall](https://note.xdq.me/centos-7-firewalldchang-yong-ming-ling/)
[Firewall](https://oracle-base.com/articles/linux/linux-firewall-firewalld)

[SSH](https://wiki.centos.org/HowTos/Network/SecuringSSH)
[SSH](http://linux.it.net.cn/CentOS/course/2016/0529/22705.html)
[SSH](http://www.cnblogs.com/nwf5d/archive/2011/03/28/1998002.html)

firewall-cmd
--permanent
--zone=public
--add-rich-rule="rule family="ipv4" source address="192.168.0.4/24" accept"

firewall-cmd --permanent --zone=public --add-rich-rule 'rule family="ipv4" source address="219.224.0.0/20" service name=ssh accept'
firewall-cmd --permanent --zone=public --remove-rich-rule 'rule family="ipv4" source address="219.224.0.0/20" service name=ssh accept'
firewall-cmd --reload
