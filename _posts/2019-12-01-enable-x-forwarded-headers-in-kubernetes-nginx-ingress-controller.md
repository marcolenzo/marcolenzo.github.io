---
title: 'Enable X-Forwarded-* Headers in Kubernetes Nginx Ingress Controller'
date: '2019-12-01'
author: marcolenzo
layout: post
permalink: /enable-x-forwarded-headers-in-kubernetes-nginx-ingress-controller/
categories:
    - 'DevOps'
tags:
    - Kubernetes
    - Kubernetes Ingress
    - Nginx
---

If you experienced some issues with your application not redirecting to HTTPs or the correct host on Kubernetes, you are not alone. While refreshing some clusters, I started experiencing this issue as well.

I immediately checked the configuration of my Load Balancer but it looked perfectly fine. After digging through the stack, I realized the issue was the [Nginx Ingress Controller](https://kubernetes.github.io/ingress-nginx/). While it had always forwarded automatically all headers it received, it stopped doing it in the new setups.

I tried to find some release notes confirming my theory with no luck. However, the solution is extremely simple. You just need to create a `ConfigMap` in the `ingress-nginx` namespace as the one shown below.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  annotations:
  labels:
    app: ingress-nginx
  name: nginx-configuration
  namespace: ingress-nginx
data:
  use-forwarded-headers: "true"
```

There are many more configuration options you can set. Have a look at the [official documentation](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/).

I hope you find my article before spending too much time solving this issue. It is quite tricky especially if you are setting up a full cluster from scratch as there are a lot of possible culprits. Whenever dealing with issues related to incorrect application redirects always make sure that your application is receiving the correct `X-Forwarded-Proto` and `X-Forwarded-Host` headers. If you have several hops, you will have to enable logs to figure out where to act.

## Video content

If you like Kubernetes and want to know more. Visit my YouTube channel as well: <https://youtube.com/@MarcoLenzo>