---
title: 'Package a Spring Boot application into a Docker container'
date: '2016-04-11'
author: marcolenzo
layout: post
permalink: /package-a-spring-boot-application-into-a-docker-container/
categories:
  - 'DevOps'
tags:
  - Docker
  - Maven
  - Java
---

In this article, I will describe how we can easily package our Spring Boot Java Application into a Docker container using the [docker-maven-plugin](https://github.com/spotify/docker-maven-plugin)

The reason why we favor containers in our Continuous Delivery (CD) prototyping is that they offer greater isolation than Virtual Machines (VMs). When the number of applications deployed on a VM grows, the chance of dependency conflicts (e.g. JDK or System Libraries) is very high. Containers are also lightweight, portable and fast to create and destroy. Lastly there are several orchestration technologies out there, like Kubernetes and Mesos, that make working with containers a breeze.

Firstly, you need to define a remote docker host by the setting the `DOCKER_HOST` environment variable on the machine you are going to execute the build.

```shell
DOCKER_HOST=tcp://192.168.0.10:2375
```

Docker by default runs on a non-networked socket and does not connect to unprotected registries. If you want to expose it over the network and have it connect to your private registry over HTTP you need to include the following options in its configuration file `/etc/sysconfig/docker`

```shell
-H=tcp://192.168.0.19:2375 --insecure-registry=192.168.0.19:5000
```

Finally, we restart docker and launch the registry

```shell
systemctl restart docker
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

Make sure to replace the IPs with the correct ones.

## Create the Dockerfile

A `Dockerfile` is a text document that contains all the commands a user could call on the command line to assemble an image. Using the `docker build` command users can create an automated build that executes several command-line instructions in succession.

Place the following `Dockerfile` in `/src/main/docker`

```Dockerfile
FROM frolvlad/alpine-oraclejdk8:slim
VOLUME /tmp
ADD checkip.jar app.jar
RUN sh -c 'touch /app.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

## Setup the Docker Maven Plugin

In the `pom.xml` add this plugin configuration

```xml
<plugin>
	<groupId>com.spotify</groupId>
	<artifactId>docker-maven-plugin</artifactId>
	<version>0.4.5</version>
	<executions>
		<execution>
			<id>build-image</id>
			<phase>package</phase>
			<goals>
				<goal>build</goal>
			</goals>
			<configuration>
				<imageName>192.168.0.19:5000/checkip:${project.version}</imageName>
				<dockerDirectory>${basedir}/src/main/docker</dockerDirectory>
				<resources>
					<resource>
						<targetPath>/</targetPath>
						<directory>${project.build.directory}</directory>
						<include>${project.build.finalName}.jar</include>
					</resource>
				</resources>
			</configuration>
		</execution>
	</executions>
</plugin>
```

That's all! Now you can run your build.

## Run you build

```shell
mvn clean package -DpushImage
```

Once the build is complete you can go on any docker enabled machine configured to connect to your registry and run your application:

```shell
docker run -d -p8080:8080 -t 192.168.0.19:5000/checkip:0.0.1-SNAPSHOT
```

## Get the sample from Git

You can get the code for this tutorial on <https://github.com/marcolenzo/spring-boot-docker-spotify>