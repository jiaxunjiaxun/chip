# Chip for code and design

## java

### Open JDK

    sudo apt-get install openjdk-8-jdk openjdk-8-doc openjdk-8-source

### Oracle JDK

#### Install

    sudo apt-get purge openjdk*
    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java [version] -installer
    
#### Management

    sudo update-alternatives --config java
    sudo update-alternatives --config javac

#### Environment Variable

    sudo nano /etc/environment
    # See Management
    JAVA_HOME="YOUR_PATH"
    source /etc/environment
    echo $JAVA_HOME

#### Other tools

    sudo apt-get install ant maven ivy groovy gradle