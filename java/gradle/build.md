# Gradle

## Step by Step

1. Download Gradle
2. Set Environment Variables
    - GRADLE_HOME: Path to Gradle
    - GRADLE_OPTS: For remote debug (-Xdebug -Xrunjdwp:transport=dt_socket,address=5005,server=y,suspend=n)
    - GRADLE_USER_HOME: Path to Gradle cache
3. Run command and start IntelliJ_IDEA

## Build tool

### Gradle and Plugin tool

- Gradle Tool: [Gradle](http://gradle.org/)
- Scaffold and Template tool: [Gradle Template](http://cjstehno.github.io/gradle-templates/)

### Gradle command

~~~ gradle
gradle build
gradle -q taskName
gradle idea
gradle jettyRun
~~~

### Fat Jetty and Tomcat

~~~ bash
gradle appRun
gradle buildProduct
~~~

[Gretty doc](http://akhikhl.github.io/gretty-doc/index.html)
[Gretty src](https://github.com/akhikhl/gretty)

[Ref](https://howtoprogram.xyz/2016/09/12/gradle-cache-location/)
