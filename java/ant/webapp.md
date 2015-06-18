# Build properties

    webapp.name=your_web_app_name
    # For Tomcat
    deploy.dir=/path/to/tomcat/vhost
    # For Resin
    deploy.dir=/path/to/resin/vhost
    
    dir.src=src
    dir.src.htm.xml=src/cn/org/china/opinion/model
    package.htm.xml=cn/org/china/opinion/model
    
    # Compile parameters
    compile.source.version=1.5
    compile.target.version=1.5
    compile.encoding=UTF-8
    compile.debug=off
    compile.debuglevel=lines,vars,source

# Build xml

    <?xml version="1.0" encoding="UTF-8"?>
    <project name="opinion" default="flush-deploy" basedir=".">
        <!-- set project properties -->
        <property file="build.properties" />
        <property name="version" value="0.1.0" />
        <property name="year" value="2009" />
        
        <!-- set path properties -->
        <property name="dist" value="." />
        <property name="src" value="src" />
        <property name="build" value="build.dest" />
        <property name="web" value="webapp" />
        <property name="lib" value="lib" />
        <property name="conf" value="conf" />
        
        <!-- make deploy dir -->
        <property name="dir.build.project" location="${build}/${webapp.name}" />
        <property name="dir.build.project.classes" location="${dir.build.project}/classes" />
        <property name="dir.build.project.store.lib" location="${dir.build.project}/lib" />
        <property name="dir.build.htm.xml" location="${dir.build.project.classes}/${package.htm.xml}" />
        <path id="project.class.path">
            <pathelement path="${java.class.path}" />
            <pathelement path="${additional.path}" />
            <fileset dir="${lib}">
                <include name="**/*.jar" />
            </fileset>
        </path>
        
        <target name="init">
            <!-- create the time stamp -->
            <tstamp/>
            <echo message="----------- ${webapp.name} ${version} [${year}] ------------" />
            <delete dir="${build}" />
            <mkdir dir="${build}" />
            <mkdir dir="${dir.build.project.classes}" />
            <mkdir dir="${dir.build.project.store.lib}" />
        </target>
        
        <target name="compile-project" depends="init">
            <!-- compile the java code from ${src} into ${build} -->
            <javac srcdir="${dir.src}" destdir="${dir.build.project.classes}" encoding="UTF-8" debug="on">
                <classpath refid="project.class.path" />
            </javac>
            <copy todir="${dir.build.htm.xml}">
                <fileset dir="${dir.src.htm.xml}" excludes="*.java" />
            </copy>
        </target>
        
        <target name="store-jar" depends="compile-project">
            <jar jarfile="${dir.build.project.store.lib}/china-${webapp.name}-${version}.jar" basedir="${dir.build.project.classes}" encoding="UTF-8" />
        </target>
        
        <target name="build-webapp" depends="store-jar">
            <copy todir="${build}/${webapp.name}">
                <fileset dir="${web}" />
            </copy>
            <copy todir="${build}/${webapp.name}/WEB-INF/lib">
                <fileset dir="${lib}" />
                <fileset dir="${dir.build.project.store.lib}" />
            </copy>
            <copy todir="${build}/${webapp.name}/WEB-INF">
                <fileset dir="${conf}" includes="*.xml *.tld" />
            </copy>
            <copy todir="${build}/${webapp.name}/WEB-INF/classes">
                <fileset dir="${conf}" excludes="*.xml" />
            </copy>
            <copy todir="bin">
                <fileset dir="${conf}" excludes="*.xml" />
            </copy>
        </target>
        
        <target name="clean">
            <delete dir="${build}" />
            <delete file="${lib}/china-${webapp.name}-${version}.jar" />
            <delete file="${web}/WEB-INF/lib/china-${webapp.name}-${version}.jar" />
        </target>
        
        <target name="flush-deploy" depends="build-webapp">
            <copy todir="${deploy.dir}/${webapp.name}">
                <fileset dir="${build}/${webapp.name}" excludes="classes/ lib/"></fileset>
            </copy>
        </target>
        
        <target name="flush-web">
            <copy todir="${deploy.dir}/${webapp.name}">
                <fileset dir="${web}"></fileset>
            </copy>
        </target>
    </project>