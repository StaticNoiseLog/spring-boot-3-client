Project
=======

Origin
------
Based on the presentation [Bootiful Spring Boot 3](https://www.devoxx.co.uk/talk/?id=11335) given by Josh Long in
2023-06.

This is [Josh's original GitHub repository](https://github.com/joshlong/bootiful-spring-boot-3).

Purpose
-------
Illustrate features of Spring 3 with documentation. This is a sibling project of spring-boot-3-service.


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

HTTP Client With HttpServiceProxyFactory
========================================

You could inject `WebClient` and work with that, but with Spring Boot 3 there is a simpler way. Instead, we provide an
interface (`CustomerClient`) with the methods we need for accessing the service. With the `@GetExchange` annotations we
tell Spring how to map these methods to invocations of the HTTP service.

The interface is then instantiated using the `HttpServiceProxyFactory` mechanism introduced with Spring 6.

This also works for RSocket.

API Gateway
-----------
As a Spring-Boot application, this app still also has a service component even though its primary function is to act as
a client of the downstream service (spring-boot-3-service Git repository).

Therefore, this app can act as an API gateway, which allows us to add things without changing the downstream service.

We implement this with `RouteLocator`. Like this we can receive requests on <http://localhost:9090/proxy> and route them
on to the downstream service.

The filters make this powerful. We can use them to modify the path from "/proxy" (API gateway) to "/customers" (on the
downstream service), specify retries, add response headers, add security, and much more.

### GraphQL

Sometimes you want to create a new view of the data that is distinct from the manifestation of that data in the
downstream service. This is the problem that Facebook wanted to solve in 2012. Various clients need different views of
the same data. Instead of building new endpoints for all requirements, GraphQL lets you get as much or as
little of the data as the client wants.

The configuration of GraphQL is in [schema.graphqls](src/main/resources/graphql/schema.graphqls). There are three
special types used to describe data in GraphQL configurations:

1. `Query` for read-only
2. `Subscription` for long-running queries. Like queries, subscriptions enable you to fetch data. Unlike queries,
   subscriptions are long-lasting operations that can change their result over time. They can maintain an active
   connection to your GraphQL server (most commonly via WebSocket), enabling the server to push updates to the
   subscription's result.)
3. `Mutation` for everything else

We enable GraphiQL (in-browser tool for writing, validating, and testing GraphQL queries) by
setting `spring.graphql.graphiql.enabled=true` in [application.properties](src/main/resources/application.properties).
GraphiQL becomes available at <http://localhost:9090/graphiql>.

The logic that supports GraphQL is implemented in the class `CustomerGraphqlController` (a Spring `@Controller`).

Here is an example query that can be entered in the GraphiQL UI:

    query {
      customers {
        id, name
      }
    }

If you had another service that returns, say, Profile data, you can aggregate the data from both services, Customer
plus Profile and use a query like this:

    query {
      customers {
        id, name
        profile {
          id
        }
      }
    }

Combining Customer and Profile data can become slow because fetching the Profile of each Customer usually requires an
additional call to the Profile service. `@BatchMapping` uses GraphQL Data Loaders behind the scenes and can be
used for a more efficient implementation.
