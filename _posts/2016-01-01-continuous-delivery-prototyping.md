---
title: 'Continuous Delivery Prototyping Step by Step'
date: '2016-01-01'
author: marcolenzo
layout: post
permalink: /continuous-delivery-prototyping/
categories:
    - 'DevOps'
tags:
    - 'Continuous Delivery'
    - 'Continuous Integration'
---

This is the first of a series of articles where we will embark on our journey to prototyping continuous delivery.

![Continuous Delivery](/assets/img/2016/01/cicd.jpg)

The approach taken is going to be very practical. My assumption is that if you are reading this article you already know about continuous delivery and its benefits. This way we can avoid getting lost in philosophical discussions.  
Our end target is to automate deployments all the way to production or any other target environment. It doesn’t really matter at this stage if this environment is production, staging or development. The most important thing is that we move with small simple steps. We gain intelligence from each step we perform and adjust our strategy accordingly. Our objective is to learn.  
This is our target pipeline.  
  
We will use open sourced or free technologies. If you are more into finding a magic tool that does it all, then this is not for you.  
Less words more action.  
At the time of writing, these should be the high level steps we need to perform. I am going to update this list as I move through the series of articles.  
Stay tuned!

- [Step 1: Microservices with Spring Boot](https://marcolenzo.eu/2016/01/08/microservices-with-spring-boot/)
- [Step 2: Continuous Integration with Git and Jenkins](https://marcolenzo.eu/2016/01/08/microservices-with-spring-boot/)
- [Step 3: Package a Spring Boot Application into a Docker Container](https://marcolenzo.eu/2016/04/11/package-a-spring-boot-application-into-a-docker-container/)
- Step 4: Orchestrating containers with Kubernetes
- Step 5: Introduce acceptance tests
- Step 6: Go Live!

\* Note this list of steps if bound to change.