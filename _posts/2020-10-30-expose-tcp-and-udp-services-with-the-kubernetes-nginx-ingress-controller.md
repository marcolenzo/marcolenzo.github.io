---
title: 'Expose TCP and UDP services with the Kubernetes Nginx Ingress Controller'
date: '2020-10-30'
author: marcolenzo
layout: post
permalink: /expose-tcp-and-udp-services-with-the-kubernetes-nginx-ingress-controller/
categories:
    - 'DevOps'
tags:
    - Kubernetes
    - Kubernetes Ingress
    - Nginx
---

This tutorial will show how to expose TCP or UDP services running in your Kubernetes cluster such as MySQL or any other database with the Nginx Ingress Controller. As you may know, the [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) caters exclusively for HTTP and HTTPS traffic. If you want to expose other kind of services, you would need to use a Service of type `NodePort` or, if supported by your cluster, `LoadBalancer`. However, it is also possible to leverage the Nginx Ingress Controller.

## Installing the Nginx Ingress Controller

If you do not have the Nginx Ingress Controller already available in your Kubernetes cluster, you can follow my tutorial on how to [Install Nginx Ingress Controller on Kubernetes or Microk8s](https://marcolenzo.eu/2020/08/04/how-to-install-nginx-ingress-controller-on-kubernetes-and-microk8s/).

## Configure TCP services

Let’s create a `ConfigMap` named `tcp-services` in the `nginx-ingress` namespace to expose a sample TCP service such as MySQL.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: tcp-services
  namespace: ingress-nginx
data:
  12345: "default/mysql:3306"
```

In the `data` section we create key-value entries where the key is the port we want to expose on our cluster and the value is the target service using the pattern `namespace/service-name:port`.

In the example above we are exposing on port `12345` a Kubernetes service in the `default` namespace, named `mysql` and accepting traffic on port `3306`.

## Configure UDP services

The procedure is identical to the one above with the only exception that we name the `ConfigMap` as `udp-services`. Let’s expose as UDP service Kube DNS.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: udp-services
  namespace: ingress-nginx
data:
  53: "kube-system/kube-dns:53"
```

In this example we are exposing on port `53` the Kubernetes service `kube-dns` available on port `53` in the `kube-system` namespace.

## Conclusion

Exposing TCP and UDP services with the Nginx Ingress Controller is extremely simple and just require the creation of `ConfigMap`s in the namespace where the ingress controller is installed. While it is possible to achieve the same target using the `Service` resource you might want to use the Nginx Ingress Controller to use some properties of this reverse proxy.