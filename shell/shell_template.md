# Shell template

[Ref](http://www.infoq.com/cn/articles/13-tips-tricks-for-writing-shell-scripts-with-awesome-ux?utm_source=infoq&utm_medium=popular_widget&utm_campaign=popular_content_list&utm_content=homepage)

## Help
~~~ bash
#!/bin/sh
if [ ${#@} -ne 0 ] && [ "${@#"--help"}" = "" ]; then
    printf -- "... some help ...\n";
    printf -- "... some help ...\n";
    printf -- "... some help ...\n";
    exit 0;
fi;
~~~

## Check command existence
~~~ shell
#!/bin/sh
_=$(command -v docker);
if [ "$?" != "0" ]; then
    printf -- "You don't seem to have Docker installed.\n";
    printf -- "Get it: https://www.docker.com/community-edition\n";
    printf -- "Exiting with code 127...\n";
    exit 127;
fi;
# ...
~~~

## Independency of directory
~~~ shell
#!/bin/sh
CURR_DIR="$(dirname $0);"
printf -- "moving application to /opt/app.jar";
mv "${CURR_DIR}/application.jar" /opt/app.jar;
~~~

## Print execution
~~~ shell
#!/bin/sh
printf -- "Downloading required document to ./downloaded... ";
wget -o ./downloaded https://some.site.com/downloaded;
printf -- "Moving ./downloaded to /opt/downloaded...";
mv ./downloaded /opt/;
printf -- "Creating symlink to /opt/downloaded...";
ln -s /opt/downloaded /usr/bin/downloaded;
~~~
