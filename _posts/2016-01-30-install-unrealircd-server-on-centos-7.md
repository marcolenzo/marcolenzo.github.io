---
title: 'Install UnrealIRCd Server on CentOS 7'
date: '2016-01-30'
author: marcolenzo
layout: post
permalink: /install-unrealircd-server-on-centos-7/
categories:
    - 'DevOps'
tags:
    - CentOS
    - UnrealIRCd
---

In this article we will install UnrealIRCd on a CentOS 7 machine. UnrealIRC is the most popular IRC server. It is written in C and it is open source.

## User Setup and Download

```
## Add User
adduser ircd

## Install required libraries
yum install make gcc openssl openssl-devel

## Download and extract UnrealIRCd.
su ircd
cd ~

wget https://www.unrealircd.org/unrealircd4/unrealircd-4.0.1.tar.gz
tar zxvf unrealircd-4.0.1.tar.gz
cd unrealircd-4.0.1.tar.gz/
```

## Compilation

It is safe to leave everything to default. However take your time during the process to get accustomed to configuration options. To navigate over the initial release notes press ENTER.

```
./Config

make
make install
```

## Configuration

Copy the example configuration and make sure to read it all and configure accordingly to your likings. While you can set most of the settings as offered by default, make sure to define the IRC Operator in the oper section. The configuration is validated at runtime and validation messages are very helpful to troubleshoot configuration issues.

```
cp conf/example.conf ~/unrealircd/conf/unrealircd.conf
vi ~/unrealircd/conf/unrealircd.conf

# Create RULES and MOTD files.
vi ~/unrealircd/conf/ircd.rules
vi ~/unrealircd/conf/ircd.motd
```

## Start the server and Enjoy!

```
cd ~/unrealircd
./unrealircd start
```

That’s all folks!