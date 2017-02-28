# Backup MySQL Database

    #!/bin/bash
    #
    # Shell script to backup MySQL database with mysqldump
    # dump all databases into separate files.
    # Author: weejar
    # create date: 2014-3-25
    # email: weejar@gmail.com
    # Copyright (C) 2014 anbob.com   

    # Get date in yyyy-dd-mm format
    TIMESTAMP=$(date +"%F")

    # MYSQL_USER="root"
    # MYSQL_PASSWORD="*******"

    #  ZIP BACKUP FILE PASSWORD
    ZIPPWD=888888

    # backup file keep in local NUM times
    NUM=3

    # Linux bin path
    MYSQL=/bin/mysql
    MYSQLDUMP=/usr/local/mysql/bin/mysqldump
    ZIP=/usr/bin/zip
    FTP=/usr/bin/ftp

    # Get HOST IP address
    IPADR=`/sbin/ip addr show eth0|grep inet|awk '{print $2}'|awk -F/ '{print $1}'`

    # Root directory where backup will be stored
    BACKUP_DIR=/data/backup/sqldump

    # Main directory where backup log will be store
    LOGFILE=${BACKUP_DIR}/mysqldump_$TIMESTAMP.log

    # Summary backup file
    BACKUPFILE=${BACKUP_DIR}/${IPADR}_Mysqldump_${TIMESTAMP}.tar

    # Ftp backup directory will be upload
    FTPDIR="/g:/backup"  

    # DBA email to  send logfile 
    EMAIL=dba@anbob.com

    # Create direcotry if $BACKUP_DIR doesn't exist.
    if [ ! -d "$BACKUP_DIR" ]; then
        mkdir -p $BACKUP_DIR
    fi

    # Redirect output to LOGFILE
    exec 1> ${LOGFILE} 2>&1

    echo  $(date +"%F %r")

    #databases=`$MYSQL -u$MYSQL_USER -p$MYSQL_PASSWORD -e "SHOW DATABASES;" | grep -Ev "(Database|information_schema)"`

    # List need backup databases
    databases=`$MYSQL  -e "SHOW DATABASES;" | grep -Ev "(Database|information_schema|performance_schema)"`

    for db in $databases; do
    echo "backup $db is running..."

    #$MYSQLDUMP --force --opt --user=$MYSQL_USER -p$MYSQL_PASSWORD --databases $db | gzip > "$BACKUP_DIR/$db.gz"

    $MYSQLDUMP --force --opt  --databases $db | $ZIP -P $ZIPPWD  > ${BACKUP_DIR}/${IPADR}_${db}_${TIMESTAMP}_encrypt.zip

    # Verify BACKUP 
    if [ $? -ne 0 ]; then
        echo "backup $db fails!"         
    else
        echo "backup $db completed!" 
    fi

    echo  $(date +"%F %r")
    echo "********************************"

    done

    # Merge backup file
    tar cvf ${BACKUPFILE} ${BACKUP_DIR}/${IPADR}_*_${TIMESTAMP}_encrypt.zip  ${LOGFILE}

    # Remove expiry backup file

    rm  ${BACKUP_DIR}/*.zip

    ls -rt ${BACKUP_DIR}/${IPADR}_*mysqldump.tar|head -n -$NUM
    echo 'deleting...'

    ls -rt ${BACKUP_DIR}/${IPADR}_Mysqldump*.tar|head -n -$NUM|xargs rmi -f

    # Upload backup file to FTP Server

    $FTP -n <<EOF 
        open 192.168.212.110
        user ftpuser ftppassword
        cd $FTPDIR
        binary
        prompt        
        put ${BACKUPFILE}
        bye
    EOF

    if [ $? -ne 0 ]; then
       echo "ftp upload backup file fails!"         
    else
       echo "ftp upload backup file successful!" 
    fi

    echo  $(date +"%F %r")
    echo "********************************"

 
    # send email ,About how to send email on linux (http://www.anbob.com/archives/1753.html)

    cat $LOGFILE | mail -s "${IPADR} backup info" $EMAIL

    # end

# Backup 10 days

[Ref](http://www.2daygeek.com/shell-script-for-mysql-database-backup-automation/)

    #!/bin/bash
    DATE=$(date +%d-%m-%Y)
    BACKUP_DIR="/backup/test-backup"
    MYSQL_USER="root"
    MYSQL_PASSWORD="***"
    MYSQL=/u01/mysql/bin/mysql
    MYSQLDUMP=/u01/mysql/bin/mysqldump
    
    # To create a new directory into backup directory location
    mkdir -p $BACKUP_DIR/$DATE
    
    # get a list of databases
    databases=`$MYSQL -u$MYSQL_USER -p$MYSQL_PASSWORD -e "SHOW DATABASES;" | grep -Ev "(Database|information_schema)"`
    
    # dump each database in separate name
    for db in $databases; do
    echo $db
    $MYSQLDUMP --force --opt --user=$MYSQL_USER -p$MYSQL_PASSWORD --databases $db | gzip > "$BACKUP_DIR/$DATE/$db.sql.gz"
    done
    
    # Delete files older than 10 days
    find $BACKUP_DIR/* -mtime +10 -exec rm {} \;