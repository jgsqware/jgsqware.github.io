---
layout: post
title: Spring JPARepository Unit Test fails
tagline: java.lang.IllegalArgumentException A ServletContext is required to configure default servlet handling
date: 2014-06-20
disqus_identifier: 2014-06-20-1
comments: True
tags: [Spring, Spring-Data-JPA, Unit Test]
---

I have a Spring MVC application using [Spring-Data-JPA](http://projects.spring.io/spring-data-jpa/) API to manage Hibernate entity.
I'm testing it by a classic Spring Test class.

My test class is like that:

{% highlight java %}
    RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(locations = {"/META-INF/Spring/applicationContext-test.xml"})
    @Transactional
    public class UnitRepositoryTest {

        @Autowired
        UnitRepository unitRepository;

        @Test
        public void testCreateUnit(){
            Unit unit = UnitFixtures.getEnglishUnit(defaultId);

            assertNull(unit.getId());

            unit = unitRepository.saveAndFlush(unit);

            assertNotNull(unit.getId());
            assertTrue(unit.getId() > 0);

        }
    }
{% endhighlight %}

Then, my JPARepository is:

{% highlight java %}
    public interface FaqRepository extends JpaRepository<Faq, Integer> {
    }
{% endhighlight %}

Finally, my *applicationContext-test.xml* configuration file for testing purpose is:

{% highlight xml %}
    <?xml version="1.0" encoding="UTF-8"?>
<!-- Hiding XMLNS declaration for lisibility -->
<beans xmlns="..." >

    <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" id="placeholderApp">
        <property name="locations">
            <list>
                <value>classpath:application.properties</value>
                <value>classpath:version.properties</value>
            </list>
        </property>
    </bean>
    <context:component-scan base-package="com.company.project">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <jpa:repositories base-package="com.company.project.repositories" />

    <bean id="entityManagerFactory"
        class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="persistenceUnitName" value="testingSetup" />
        <property name="jpaVendorAdapter">
            <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter" />
        </property>

        <property name="jpaProperties">
            <props>

                <prop key="hibernate.dialect">org.hibernate.dialect.HSQLDialect</prop>
                <prop key="hibernate.show_sql">true</prop>
                <prop key="hibernate.format_sql">false</prop>
                <prop key="hibernate.hbm2ddl.auto">create-drop</prop>
            </props>
        </property>
    </bean>

    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName">
            <value>org.hsqldb.jdbcDriver</value>
        </property>
        <property name="url">
            <value>jdbc:hsqldb:mem:test2db;sql.syntax_mys=true</value>
        </property>
        <property name="username">
            <value>sa</value>
        </property>
        <property name="password">
            <value></value>
        </property>
    </bean>

    <!-- Enable Spring Transaction Manager with Annotations -->
    <tx:annotation-driven transaction-manager="transactionManager"/>

    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
        <property name="entityManagerFactory" ref="entityManagerFactory" />
    </bean>

    <bean id="applicationBeanContext" class="com.company.project.context.ApplicationBeanContext" >
        <property name="version" value="${cfg.project.version}" />
    </bean>

</beans>

{% endhighlight %}

On deployed version, all is fine. But, when I'm running test case, I code this exception:

{% highlight java %}
    java.lang.IllegalArgumentException: A ServletContext is required to configure default servlet handling
{% endhighlight %}

It's quite strange.
Ok, I can understand I'm in a WebMVC context, but the test case is only applying on the Entity layer.

But by adding the annotation `@WebAppConfiguration` to the test class, everything run fine...

I don't why Spring need the WebAppConfiguration to run this test class, but if someone have an idea, i'm open to the discussion.
