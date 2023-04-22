---
title: 'Install Maven on Ubuntu 22.04'
date: '2022-12-09'
author: marcolenzo
layout: post
permalink: /install-maven-on-ubuntu-22-04/
categories:
    - 'DevOps'
tags:
    - Java
    - Maven
    - Ubuntu
---

In this tutorial, we install the latest version of Maven on Ubuntu 22.04. Unfortunately, the Ubuntu repository ships an old version of Maven which is not compatible with the recent versions of the Java Development Kit (JDK). We will proceed with the manual installation.

## Pre-requisites

Before proceeding with the Maven installation, **we need to have Java already available on our machine**. If you do not have it yet, have a look at this other post of mine: [Install Java Temurin JDKs instead of OpenJDK](https://marcolenzo.eu/install-java-temurin-jdks-instead-of-openjdk/). It will take just a minute. Once you complete that tutorial come back and we can proceed further.

## Installation

We are going to download the latest version of Maven which is `3.8.6` at the time of writing. I will try to keep this post updated but if you want to check quickly what is the latest version now, just head over to <https://maven.apache.org/download.cgi>. Update the code snippet below with the target version if you find a newer one.

```shell
MAVEN_VERSION=3.8.6
wget https://downloads.apache.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz
```

Once we double check that the file `apache-maven-3.8.6-bin.tar.gz` was downloaded, we extract it and move it under the `/opt` directory.

```shell
tar -xvzf apache-maven-3.8.6-bin.tar.gz
sudo cp -r apache-maven-3.8.6 /opt/maven
```

We are almost there! Next step, we configure the environment variables for Maven by creating a file named `/etc/profile.d/maven.sh`. **A very important variable is the `JAVA_HOME`** and since it can change from one system to another we need to find first where it is located. We can use `java` itself to show the settings and fetch the `java.home` property.

```shell
marcol@tutorials:~$ java -XshowSettings:properties -version | grep home
Property settings:
    file.encoding = UTF-8
    file.separator = /
    java.class.path =
    java.class.version = 61.0
    java.home = /usr/lib/jvm/temurin-17-jdk-amd64
...
```

In my case the `JAVA_HOME` must be set to `/usr/lib/jvm/temurin-17-jdk-amd64` and this is the environment variable file I create for Maven.

```shell
sudo tee /etc/profile.d/maven.sh > /dev/null << 'EOF'
export JAVA_HOME=/usr/lib/jvm/temurin-17-jdk-amd64
export M2_HOME=/opt/maven
export MAVEN_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
EOF
```

Finally, we make the file executable and source it, so we can add `mvn` to the `PATH`.

```shell
sudo chmod +x /etc/profile.d/maven.sh
source /etc/profile.d/maven.sh
```

And voilà! We got the latest version of Maven available on our environment!

```shell
marcol@tutorials:~$ mvn -version
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /opt/maven
Java version: 17.0.5, vendor: Eclipse Adoptium, runtime: /usr/lib/jvm/temurin-17-jdk-amd64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.15.0-56-generic", arch: "amd64", family: "unix"
```

![Too Easy](/assets/img/2022/12/too-easy.jpg)

## Conclusion

It takes very little to install the latest version of Maven manually. If you want me to create any other tutorial, let me know in the comments sections.

Bye bye!