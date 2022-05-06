# Chip for code and design

## java

### Open JDK

``` bash
sudo apt-get install openjdk-8-jdk openjdk-8-doc openjdk-8-source
```

### Oracle JDK

#### Install

``` bash
# Use deb file
sudo apt-get purge openjdk*
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java [version] -installer
```

``` bash
# Use archive file
# Download java from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
# Unzip package
export JAVA_HOME=/path/to/jdk_{version}
export PATH=$JAVA_HOME/bin:$PATH
```

#### Management

``` bash
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

#### Environment Variable

``` bash
# ---- Add environment variables ----
# /etc/environment or /etc/profile
sudo vim /etc/environment
sudo vim /etc/profile

# environment or profile
JAVA_HOME=/path/to/jdk
JRE_HOME=$JAVA_HOME/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH

source /etc/environment
source /etc/profile

# jdk 11+ no jre
# jlink [options] --module-path module_path --add-modules module [, module …]

# default module
java.base                   java.compiler
java.datatransfer           java.desktop
java.instrument             java.logging
java.management             java.management.rmi
java.naming                 java.net.http
java.prefs                  java.rmi
java.scripting              java.se
java.security.jgss          java.security.sasl
java.smartcardio            java.sql
java.sql.rowset             java.transaction.xa
java.xml.crypto             java.xml
jdk.accessibility           jdk.aot
jdk.attach                  jdk.charsets
jdk.compiler                jdk.crypto.cryptoki
jdk.crypto.ec               jdk.crypto.mscapi
jdk.dynalink                jdk.editpad
jdk.hotspot.agent           jdk.httpserver
jdk.internal.ed             jdk.internal.jvmstat
jdk.internal.le             jdk.internal.opt
jdk.internal.vm.ci          jdk.internal.vm.compiler
jdk.internal.vm.compiler.management
jdk.jartool                 jdk.javadoc
jdk.jcmd                    jdk.jconsole
jdk.jdeps                   jdk.jdi
jdk.jdwp.agent              jdk.jfr
jdk.jlink                   jdk.jshell
jdk.jsobject                jdk.jstatd
jdk.localedata              jdk.management.agent
jdk.management.jfr          jdk.management
jdk.naming.dns              jdk.naming.rmi
jdk.net                     jdk.pack
jdk.rmic                    jdk.scripting.nashorn
jdk.scripting.nashorn.shell jdk.sctp
jdk.security.auth           jdk.security.jgss
jdk.unsupported.desktop     jdk.unsupported
jdk.xml.dom                 jdk.zipfs

jlink --module-path jmods --add-modules java.base,java.desktop --output /path/to/jre

```

#### Other tools

``` bash
sudo apt-get install ant maven ivy groovy gradle
```

## Ref
[Java Guide](https://github.com/Snailclimb/JavaGuide)
[](http://javatutorialhq.com/)
[](https://blog.csdn.net/weixiaodedao/article/details/106710091)