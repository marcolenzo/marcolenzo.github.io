---
title: 'Microservices with Spring Boot'
date: '2016-01-08'
author: marcolenzo
layout: post
permalink: /microservices-with-spring-boot/
categories:
  - DevOps
tags:
  - Microservices
  - 'Spring Boot'
  - Java
---

In this article, we will learn how to build a microservice in minutes with Spring Boot.

The first step in our continuous delivery pipeline is the development phase.

If we were allowed to start with a blank sheet, I would strongly suggest to consider a microservice architecture. Its core concept is to split the system in individually deployable services with very specific scope. From a continuous delivery perspective having smaller services allows to reduce the implementation and deployment time. The team can perform incremental updates with more confidence knowing that the realm of their changes is limited to their specific service. Great!

Another advantage of a microservice architecture is that it fosters the creation of cross functional teams. In contrast, multi-tiered or multi-layered architectures usually result in teams confined within the layers of the system. The main difference is that a cross functional team is able to implement a feature independently while specialized teams needs to eventually integrate their work.

Unfortunately, starting the planning of a continuous delivery pipeline from a blank sheet is a privilege for the few. The reality is that we often need to deal with existent systems which might be complex, difficult to build, deploy and release. In these scenarios, I would still suggest to slowly migrate to microservice or service oriented architectures by breaking any monolith in independent services. The reality is that deployment automation is impossible or very hard if you need to deal with a very heavy application which needs hours to build, is difficult to test and slow to deploy and start.

Anyway, I guess it’s time to code! Let’s build our first microservice.

To keep things simple we will write a service which returns the remote address of the client performing the request. A similar service is offered by amazon: <http://checkip.amazonaws.com/>

If you happen to code in Java, Spring has made your life incredibly easy. Just access the [Spring Initializr website](https://start.spring.io/) and create a new Spring Boot application with the `Web` dependency. Or if you prefer, clone my [spring-boot-samples repository](https://github.com/marcolenzo/spring-boot-samples).

All we need are three files.

Firstly we define our Maven Project Object Model where we set `spring-boot-starter-parent` as parent. We define `spring-boot-starter-web` as dependency. Finally we also need to define the `spring-boot-maven-plugin` which will let us package our application in a fat JAR.

### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.marcolenzo.checkip</groupId>
	<artifactId>checkip</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>
	<name>checkip</name>
	<description>Sample Spring Boot Application offering Check IP service</description>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.3.1.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>
```

We then create the launcher class. The `@SpringBootApplication` annotation will trigger auto-configuration of your `ApplicationContext`.

### CheckIpApplication.java

```java
package com.marcolenzo.checkip;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class CheckIpApplication {
	public static void main(String[] args) {
		SpringApplication.run(CheckIpApplication.class, args);
	}
}
```

### CheckIpController.java

```java
package com.marcolenzo.checkip;
import javax.servlet.http.HttpServletRequest;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;
@RestController
public class CheckIpController {
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String checkIp(HttpServletRequest request) {
		return request.getRemoteAddr();
	}
}
```

Ready! We can now launch application from our IDE (IntelliJ or Eclipse) or maven as shown below. By default the server will run on port `8080`. If you intend to change the port simply define the `server.port` property in the `application.properties` file on `application.yml` one.

```
[marcol@localhost checkip]$ mvn spring-boot:run
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building checkip 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[...]
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.3.1.RELEASE)
[...]
```
Last thing we check if it actually works.

marcol@ubuntu:~$ curl http://192.168.1.113:8080
192.168.1.111

Success!

```

In next article we will discuss Continuous Integration.