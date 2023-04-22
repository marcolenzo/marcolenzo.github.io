---
title: 'Install Java Temurin JDKs instead of OpenJDK'
date: '2022-12-09'
author: marcolenzo
layout: post
permalink: /install-java-temurin-jdks-instead-of-openjdk/
categories:
    - 'DevOps'
tags:
    - Java
    - Temurin
    - JDK
    - OpenJDK
---

The OpenJDK Docker image (`openjdk`) has been [deprecated](https://hub.docker.com/_/openjdk), thus you need to immediately migrate to another distribution if you wish to receive feature and security updates. The Eclipse Temurin are a set of Java JDKs offered by [Adoptium](https://adoptium.net/about/). In this article, I show you how to install Temurin on different distribution such as Debian, Ubuntu and Alpine.

![Temurin](/assets/img/2022/12/temurin.jpg)

## Install Temurin on Debian or Ubuntu

We ensure the pre-requisite packages are present.

```shell
sudo apt install -y wget apt-transport-https
```

We download the Eclipse Adoptium GPG key and configure its `apt` repository.

```shell
mkdir -p /etc/apt/keyrings
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
```

Finally, we can update `apt` and install our preferred version of Temurin. I like to use the latest available LTS release which at the time of writing is version 17.

```shell
sudo apt update
sudo apt install temurin-17-jdk
```

We did it! We can now verify that we have the target version of `java` available on our box.

```shell
marcol@tutorials:~$ java -version
openjdk version "17.0.5" 2022-10-18
OpenJDK Runtime Environment Temurin-17.0.5+8 (build 17.0.5+8)
OpenJDK 64-Bit Server VM Temurin-17.0.5+8 (build 17.0.5+8, mixed mode, sharing)
```

## Install Temurin on Alpine

Alpine Linux is a security-oriented and lightweight Linux distribution which is commonly used in containers. Installing Temuring on Alpine is very easy. First, we download the Eclipse Adoptium RSA Key.

```shell
wget -O /etc/apk/keys/adoptium.rsa.pub https://packages.adoptium.net/artifactory/api/security/keypair/public/repositories/apk
```

Then, we install the Eclipse Adoptium APK repository.

```shell
echo 'https://packages.adoptium.net/artifactory/apk/alpine/main' >> /etc/apk/repositories
```

Finally, we install the Temurin version we prefer. As I already mentioned, I like to use the latest LTS release available, so we will choose version 17.

```shell
apk add temurin-17-jdk
```

We do a quick check to verify that `java` is available on the `PATH` and that it is the desired version.

```shell
/ # java -version
openjdk version "17.0.5" 2022-10-18
OpenJDK Runtime Environment Temurin-17.0.5+8 (build 17.0.5+8)
OpenJDK 64-Bit Server VM Temurin-17.0.5+8 (build 17.0.5+8, mixed mode, sharing)
```

## Installation on CentOS / RHEL / Fedora

You can follow the instruction provided by Adoptium [here](https://adoptium.net/installation/linux/).