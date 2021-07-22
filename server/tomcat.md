# Tomcat

## Virtual Host
``` xml
<Host name="www.sample.com" appBase="/path/to/webapp">
    <Context path="" docBase="." debug="0" />
</Host>
<Host name="www.sample.com" appBase="/path/to/webapps"
    unpackWARs="true" autoDeploy="true"
    xmlValidation="false" xmlNamespaceAware="false">
    <Context path="" docBase="/path/to/webapp" debug="0" reloadable="true" />
</Host>
```

## About Tomcat new File ACL
``` bash
# /path/to/tomcat/bin/catalina.sh
# Set UMASK unless it has been overridden
if [ -z "$UMASK" ]; then
    UMASK="0027"
fi
umask $UMASK

# ---

# /etc/profile
if [ $UID -gt 199 ] && [ "`/usr/bin/id -gn`" = "`/usr/bin/id -un`" ]; then
    umask 002
else
    umask 022
fi
```
