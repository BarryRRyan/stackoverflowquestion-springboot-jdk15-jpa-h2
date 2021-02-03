# Spring Boot + JDK9 Modules + H2 + Intellij Community Issue Replication

## Original Question
I'm trying to learn Spring-Boot with JPA and a H2 embedded db.
I created a repo that replicates my issue [available on github](https://github.com/BarryRRyan/stackoverflowquestion-springboot-jdk15-jpa-h2).
It's based on a tutorial; source found [here](https://github.com/spring-guides/gs-accessing-data-jpa.git.).

My end-state is to run this as a single-module project with all the following methods:
* mvn spring-boot:run
* Run the jar from target in Intellij
* Run the main() method within Intellij

Currently the main() method does something different to the other two approaches, but I don't know why.

If I remove the module-info.java file then all methods exit 0. I know I could ignore it but I'm trying to learn here,
so I want to know what's going on.

Intellij Build Info:
> IntelliJ IDEA 2020.3.2 (Community Edition) Build #IC-203.7148.57,
> built on January 26, 2021

With jdk 15 set but no module-info.java:

- Run as Intellij Run config. Exit code is 0.
- Run with mvn spring-boot:run. Exit code is 0.
- Run the jar. Exit code is 0.

Add the following module-info.java file to /src/main/java:

    module demo {
        exports com.example.accessingdatajpa;
        opens com.example.accessingdatajpa;
    
        requires org.slf4j;
        requires spring.boot;
        requires spring.boot.autoconfigure;
        requires spring.context;
        requires spring.data.commons;
        requires java.persistence;
        requires java.sql;
    }

Run the application by running main() in intellij and get exit 1:
> c.e.a.AccessingDataJpaApplication        : Starting AccessingDataJpaApplication using Java 15.0.2 on ...
>
> c.e.a.AccessingDataJpaApplication        : No active profile set, falling back to default profiles: default
>
> .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
>
> .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 166 ms. Found 1 JPA repository interfaces.
>
> o.s.j.d.e.EmbeddedDatabaseFactory        : Starting embedded database: url='jdbc:h2:mem:3cb47b25-69e1-4122-bc0f-69447d509171;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=false', username='sa'
>
> s.c.a.AnnotationConfigApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Invocation of init method failed; nested exception is javax.persistence.PersistenceException: Unable to resolve persistence unit root URL
>
> o.s.j.d.e.EmbeddedDatabaseFactory        : Shutting down embedded database: url='jdbc:h2:mem:3cb47b25-69e1-4122-bc0f-69447d509171;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=false'
>

Run as a jar or with mvn spring-boot:run and get exit code 0:
> ... c.e.a.AccessingDataJpaApplication        : Starting AccessingDataJpaApplication v0.0.1-SNAPSHOT using Java 15.0.2 on ...
> ... c.e.a.AccessingDataJpaApplication        : No active profile set, falling > back to default profiles: default
>
> ... .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA
> repositories in DEFAULT mode.
>
> ... .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository
> scanning in 126 ms. Found 1 JPA repository interfaces.
>
> ... com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Started.
>
> ... o.hibernate.jpa.internal.util.LogHelper  : HHH000204: Processing
> PersistenceUnitInfo [name: default]
>
Any help would be greatly appreciated.