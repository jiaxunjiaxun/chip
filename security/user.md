# User

[Ref-1](http://www.cyberciti.biz/faq/howto-linux-add-user-to-group/)
[Ref-2](http://www.cyberciti.biz/faq/howto-linux-add-user-use-adduser-command/)

## configure file

1. /etc/passwd – Contains one line for each user account.
2. /etc/shadow – Contains the password information in encrypted formatfor the system’s accounts and optional account aging information.
3. /etc/group – Defines the groups on the system.
4. /etc/default/useradd – This file contains a value for the default group, if none is specified by the useradd command.
5. /etc/login.defs – This file defines the site-specific configuration for the shadow password suite stored in /etc/shadow file.

## Add group

    groupadd <group-name>

## Change group

    usermod -a -G ftp tony
    
    usermod -g www tony

## Add user

    useradd -G <group-name> <username>
    useradd <username>
    passwd <username>

