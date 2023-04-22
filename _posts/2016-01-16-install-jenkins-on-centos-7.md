---
title: 'Install Jenkins on CentOS 7'
date: '2016-01-16'
author: marcolenzo
layout: post
permalink: /install-jenkins-on-centos-7/
categories:
    - DevOps
tags:
    - CentOS
    - Jenkins
    - Java
---

Install Jenkins in few steps on CentOS 7.

Java is a pre-requisite for your Jenkins installation. If you don’t have it, go ahead and install it. Note that Jenkins is incompatible with the GCJ version of Java. Best options are OpenJDK and the official one.

```shell
# Install Java
yum install java-1.8.0-openjdk

# Install Jenkins
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum install jenkins

# Setup the firewall to allow connections on port 8080
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --reload

# Create SSH Keys for the Jenkins User:
sudo su -u jenkins
ssh-keygen

# Setup Jenkins as a service:
systemctl start jenkins
systemctl status jenkins
```
```
● jenkins.service - LSB: Jenkins Continuous Integration Server
Loaded: loaded (/etc/rc.d/init.d/jenkins)
Active: active (running) since Sat 2016-01-16 00:11:11 GMT; 9s ago
Docs: man:systemd-sysv-generator(8)
Process: 2452 ExecStart=/etc/rc.d/init.d/jenkins start (code=exited, status=0/SUCCESS)
CGroup: /system.slice/jenkins.service
└─2467 /etc/alternatives/java -Dcom.sun.akuma.Daemon=daemonized -Djava.awt.headless=true -DJENKINS_HOME=/var/lib/j...
Jan 16 00:11:09 localhost.localdomain systemd[1]: Starting LSB: Jenkins Continuous Integration Server...
Jan 16 00:11:10 localhost.localdomain runuser[2453]: pam_unix(runuser:session): session opened for user jenkins by (uid=0)
Jan 16 00:11:11 localhost.localdomain jenkins[2452]: Starting Jenkins [ OK ]
Jan 16 00:11:11 localhost.localdomain systemd[1]: Started LSB: Jenkins Continuous Integration Server.
```

Happiness! You should be able to access Jenkins GUI on port 8080.

![Jenkins Welcome Page](/assets/img/2016/01/jenkins-welcome.jpg)

## Additional Setup through GUI

Access the `Configure System` section accessible through the `Manage Jenkins` link on the home page. Setup JDK and Maven.

![Jenkins JDK Configuration](/assets/img/2016/01/jenkins-jdk.jpg)

Setup Jenkins credentials in the `Manage Credentials` section:

![Jenkins Credentials Configuration](/assets/img/2016/01/jenkins-credentials.jpg)