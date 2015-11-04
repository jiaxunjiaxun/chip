# start and restart

    /path/to/mysql/bin/mysqladmin -u [user] -p[password] shutdown
    /path/to/mysql/bin/mysqld_safe --defaults-file=/path/to/my.cnf --user=root &