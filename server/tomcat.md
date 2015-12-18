# Tomcat

## Virtual Host

    <Host name="www.sample.com" appBase="/path/to/webapp">
        <Context path="" docBase="." debug="0" />
    </Host>
    <Host name="www.sample.com" appBase="/path/to/webapps"
        unpackWARs="true" autoDeploy="true"
        xmlValidation="false" xmlNamespaceAware="false">
        <Context path="" docBase="/path/to/webapp" debug="0" reloadable="true" />
    </Host>
