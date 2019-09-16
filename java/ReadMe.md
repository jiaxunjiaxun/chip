# Chip for code and design

## java

### Open JDK

~~~ shell
sudo apt-get install openjdk-8-jdk openjdk-8-doc openjdk-8-source
~~~

### Oracle JDK

#### Install

~~~ shell
sudo apt-get purge openjdk*
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java [version] -installer
~~~
---
~~~ shell
download java from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
unzip package
export JAVA_HOME=/home/ubuntu/java/jdk1.7
export PATH=$JAVA_HOME/bin:$PATH
~~~

#### Management

~~~ shell
sudo update-alternatives --config java
sudo update-alternatives --config javac
~~~

#### Environment Variable

~~~ shell
sudo nano /etc/environment
# See Management
JAVA_HOME="YOUR_PATH"
source /etc/environment
echo $JAVA_HOME
~~~

~~~ shell
sudo vim /etc/profile

JAVA_HOME="PATH_TO_JAVA"
JRE_HOME=$JAVA_HOME/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH

source /etc/profile
~~~

#### Other tools

~~~ shell
sudo apt-get install ant maven ivy groovy gradle
~~~

## Ref
[Java Guide](https://github.com/Snailclimb/JavaGuide)
[](http://javatutorialhq.com/)