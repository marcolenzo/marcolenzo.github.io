---
title: 'Continuous Integration with Git and Jenkins'
date: '2016-01-16'
author: marcolenzo
layout: post
permalink: /continuous-integration-with-git-and-jenkins/
categories:
    - DevOps
tags:
    - 'Continuous Integration'
    - Jenkins
    - Git
---

Continuous Integration (CI) defines a software development process where the source code is pushed by developers to a central repository and verified by automated builds. Its main aim is to detect issues as early as possible and avoid integration hell. It is a fundamental practice in any software development team.

In this article, we are going to create a CI server running Jenkins. We will use Git as our Version Control System (VCS).

While this setup comes at no cost, it can easily scale to enterprise level and can accommodate any kind of team.  
To simplify things I will be using Github as our Git Server. If you intend to host on-premise your Git Server consider the following options:

\* [Github Enteprise](https://enterprise.github.com/home)  
\* [Bitbucket Server](https://bitbucket.org/product/server)  
\* [GitLab Enteprise](https://about.gitlab.com/features/#enterprise)

Before we can proceed we need to install Jenkins on our server. Refer to the following guide:

\* [Install Jenkins on CentOS 7](https://marcolenzo.eu/2016/01/16/install-jenkins-on-centos-7/)

The CI server needs also to have Git installed. Simply run run ` sudo yum install git`

## Jenkins Job Setup

Now that we have our CI server fully configured, we can start creating a Jenkins Job which will trigger a Maven build every time we commit on our Git repository. At this stage we are going to automate builds for the `master` branch only. In a separate article I will show you how to automate builds for any branch without having to configure Jenkins manually each time.  
  
I am going to use my repository <https://github.com/marcolenzo/spring-boot-samples> as reference. Replace this repository with your own. If you don’t have one, fork my repository on Github and use your own fork.

### Step 1: Create a new Maven Job

Create a new Maven Job and name it SpringBootSamples-master.

### Step 2: Configure access to Git Repository

Under source code management, select the Git option and provide the SSH address to the repository. Make sure to add the Jenkins SSH Key to your Github account, otherwise you will be denied access.

![Jenkins Source Code Management](/assets/img/2016/01/jenkins-source-code-management.jpg)

### Step 3: Configure build triggers

You have two options at this stage. You can either opt for a polling mechanism or push through webhooks.  
Polling is very simple. Just select `Poll SCM` under the build trigger and set a cron expression. For example, if you wish to poll every minute, add the following: `H/1 * * * *`

Another option is have Github inform Jenkins whenever a commit is pushed. For this setup you need the `Github Plugin` for Jenkins. Then you can either let it configure the webhooks for you, or you can configure them manually in the settings section of your repository. The webhook should point to your Jenkins installation followed by `/github-webhook`. For instance, `https://jenkins.marcolenzo.eu/webhook-plugin`

![Jenkins GitHub Webhook](/assets/img/2016/01/github-webhook.jpg)

That’s all!

Commit something and Jenkins will take care of the rest 🙂

![Jenkins Build Status](/assets/img/2016/01/jenkins-build-status.jpg)