# MySQL (MariaDB)

## Common

    use database;
    set names utf8;
    flush privileges;

## User

    show grants for 'user'@'host';
    set password for 'user'@'host' = password('password');

### Grant user

    -- grant all
    grant all privileges on db_name.tb_name(*.*) to 'username'@'hostname(ip, %, localhost)' identified by 'password' with grant option;
    grant all on *.* to 'user'@'host';
    -- grant database
    grant all on db.* to 'user'@'host';
    grant all on db.* to 'user'@'host' identified by 'password';
    -- grant privileges, table, and column
    grant select, insert on db.* to 'user'@'host';
    grant select, insert on db.tbl to 'user'@'host';
    grant select (col1), insert(col1,col2) on db.tbl to 'user'@'host';

### Revoke user

    -- revoke user;
    revoke all privileges, grant option from 'user'@'host';
    revoke privileges on dbname[.tbname] from username;
    revoke drop on db.* from 'user'@'host';

### Drop user
    drop user 'user'@'host';

### mysql 8.0
``` shell
sudo apt-get purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*
sudo rm -rf /etc/mysql /var/lib/mysql
sudo apt-get autoremove
sudo apt-get autoclean
```

create user

``` sql
alter user 'root'@'localhost' identified with mysql_native_password by '123456';

create user 'root'@'%' identified with mysql_native_password by '123456';
grant all on *.* to 'root'@'%';
flush privileges;
```