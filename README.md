Project
=======

Origin
------
Based on the presentation [Bootiful Spring Boot 3](https://www.devoxx.co.uk/talk/?id=11335) given by Josh Long in
2023-06.

This is [Josh's original GitHub repository](https://github.com/joshlong/bootiful-spring-boot-3).

Purpose
-------
Illustrate features of Spring 3 with documentation. Sibling project of spring-boot-3-service.


spring initializr
=================

Settings
--------

- Project: Gradle-Kotlin
- Language: Java
- Spring Boot: 3.1.0
- Group: com.staticnoiselog
- Artifact: spring-boot-3-client [name of ZIP archive to download, name of main directory]
- Name: Spring Boot 3 Client
- Description: Demo Project Spring Boot 3
- Package name: com.staticnoiselog.spring-boot-3-client [suggested, fixed automatically]  
  *The original package name 'com.staticnoiselog.spring-boot-3-client' is invalid and this project uses '
  com.staticnoiselog.springboot3client' instead.*
- Packaging: Jar
- Java: 17 [Spring 3 requires Java 17 or higher and is based on Spring 6]

Dependencies
------------

- Spring Reactive Web [Web]  
  Build reactive web applications with Spring WebFlux and Netty.
- Spring for GraphQL [Web]  
  Build GraphQL applications with Spring for GraphQL and GraphQL Java.
- Gateway [Spring Cloud Routing]  
  Provides a simple, yet effective way to route to APIs and provide cross cutting concerns to them such as security,
  monitoring/metrics, and resiliency.
- GraalVM Native Support [Developer Tools]  
  Support for compiling Spring applications to native executables using the GraalVM native-image compiler.
