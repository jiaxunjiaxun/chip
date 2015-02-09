# Datasource

## jdbc properties

jdbc properties file

> datasource.url=jdbc:mysql://127.0.0.1:3306/[database_schema]?autoReconnect=true&failOverReadOnly=false&useUnicode=true&characterEncoding=utf-8
> datasource.username=[database_username]
> datasource.password=[database_password]
> datasource.driverClassName=com.mysql.jdbc.Driver

## c3p0

    <bean id="jdbcProperties" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="placeholderPrefix" value="${"/>
        <property name="location" value="classpath:config/jdbc.properties"/>
        <property name="properties">
            <props>
                <prop key="datasource.minPoolSize">5</prop>
                <prop key="datasource.maxPoolSize">30</prop>
                <prop key="datasource.initialPoolSize">10</prop>
                <prop key="datasource.maxIdleTime">60</prop>
                <prop key="datasource.acquireIncrement">5</prop>
                <prop key="datasource.numHelperThreads">5</prop>
                <prop key="datasource.checkoutTimeout">10000</prop>
            </props>
        </property>
    </bean>
    
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource" destroy-method="close">
		<property name="driverClass" value="${datasource.driverClassName}"/>
		<property name="jdbcUrl" value="${datasource.url}"/>
		<property name="user" value="${datasource.username}"/>
		<property name="password" value="${datasource.password}"/>
		<property name="minPoolSize" value="${datasource.minPoolSize}"/>
		<property name="maxPoolSize" value="${datasource.maxPoolSize}"/>
		<property name="initialPoolSize" value="${datasource.initialPoolSize}"/>
		<property name="maxIdleTime" value="${datasource.maxIdleTime}"/>
		<property name="acquireIncrement" value="${datasource.acquireIncrement}"/>
		<property name="numHelperThreads" value="${datasource.numHelperThreads}"/>
		<property name="checkoutTimeout" value="${datasource.checkoutTimeout}"/>
	</bean>

## dbcp

    <bean id="jdbcProperties" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="placeholderPrefix" value="${"/>
		<property name="location" value="classpath:config/jdbc.properties"/>
		<property name="properties">
			<props>
				<prop key="datasource.initialSize">5</prop>
				<prop key="datasource.maxActive">10</prop>
				<prop key="datasource.maxIdle">5</prop>
				<prop key="datasource.poolPreparedStatements">true</prop>
				<prop key="datasource.defaultAutoCommit">false</prop>
				<prop key="datasource.validationQuery">select now()</prop>
				<prop key="datasource.defaultTransactionIsolation">2</prop>
			</props>
		</property>
	</bean>
    
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
		<property name="driverClassName" value="${datasource.driverClassName}"/>
		<property name="url" value="${datasource.url}"/>
		<property name="username" value="${datasource.username}"/>
		<property name="password" value="${datasource.password}"/>
		<property name="initialSize" value="${datasource.initialSize}"/>
		<property name="maxActive" value="${datasource.maxActive}"/>
		<property name="maxIdle" value="${datasource.maxIdle}"/>
		<property name="defaultAutoCommit" value="${datasource.defaultAutoCommit}"/>
		<property name="defaultTransactionIsolation" value="${datasource.defaultTransactionIsolation}"/>
		<property name="poolPreparedStatements" value="${datasource.poolPreparedStatements}"/>
		<property name="validationQuery" value="${datasource.validationQuery}"/>
	</bean>

# Session

## Hibernate

	<bean id="sessionFactory" class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
		<property name="dataSource" ref="dataSource"/>
		<property name="configLocation" value="classpath:config/hibernate.cfg.xml"/>
	</bean>

# Transaction

## Hibernate

	<bean id="transactionManager" class="org.springframework.orm.hibernate4.HibernateTransactionManager">
		<property name="sessionFactory" ref="sessionFactory"/>
	</bean>

## Jdbc Template

	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>
	</bean>