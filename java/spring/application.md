# Application

## Annotation

~~~ xml
<context:component-scan base-package="[package]"/>
<context:annotation-config/>
<tx:annotation-driven/>
<mvc:annotation-driven/>
~~~

## Template

### Resource

~~~ xml
<mvc:resources mapping="/resource/**" location="/resource/"/>
~~~

### Simple JSP

path: /views/

~~~ xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<property name="prefix" value="/views/"/>
	<property name="suffix" value=".jsp"/>
</bean>
~~~

### FreeMarker

path: /views/

~~~ xml
<bean id="freeMarkerConfig" class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
	<property name="templateLoaderPaths" value="/views/"/>
	<property name="defaultEncoding" value="UTF-8"/>
</bean>
<bean id="viewResolver" class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">
	<property name="contentType" value="text/html;charset=UTF-8"/>
	<property name="prefix" value=""/>
	<property name="suffix" value=".ftl"/>
</bean>
~~~

### Global Exception

~~~ xml
<bean id="exceptionResolver" class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
	<property name="defaultErrorView" value="/error/error"/>
	<property name="defaultStatusCode" value="500"/>
	<property name="warnLogCategory" value="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver"/>
</bean>
~~~
