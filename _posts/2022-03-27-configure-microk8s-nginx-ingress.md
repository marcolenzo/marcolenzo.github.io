---
title: 'Configure microk8s NGINX Ingress'
date: '2022-03-27'
author: marcolenzo
layout: post
permalink: /configure-microk8s-nginx-ingress/
categories:
    - 'DevOps'
tags:
    - Kubernetes
    - Kubernetes Ingress
    - MicroK8s
    - Nginx
---

This is a very simple and short tutorial which helps you configure your NGINX Ingress on microk8s. A common use case is enabling the [use-forwarded-headers](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/#use-forwarded-headers) flag so that NGINX passes incoming `X-Forwarded-*` headers to upstreams.

## Verify Addon Presence

Let’s verify that the our [ingress addon](https://microk8s.io/docs/addon-ingress) is enabled and running with the following commands.

```shell
microk8s enable ingress
microk8s.kubectl get pods -A | grep ingress
```

We wait for the ingress pod to be in a Running state. Notice that the pod is running in the `ingress` namespace. We will use this information later on.

```shell
ingress        nginx-ingress-microk8s-controller-8c2bd    1/1     Running   
```

We now check if we can hit nginx.

```shell
curl -v 127.0.0.1
```

Ignore the 404 code returned and just check that nginx is the server replying to your request.

```html
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

## Find ConfigMaps

We now want to find which configuration maps were setup for the NGINX Ingress.

```shell
microk8s.kubectl -n ingress get configmaps
```

We expect an output similar to the one below.

```shell
NAME    DATA  AGE
kube-root-ca.crt                                              1           4d11h
nginx-load-balancer-microk8s-conf             0          4d11h
nginx-ingress-tcp-microk8s-conf                  0          4d11h
nginx-ingress-udp-microk8s-conf                0          4d11h
ingress-controller-leader                               0          4d11h
```

We have three ConfigMaps already setup.  
  
`nginx-load-balancer-microk8s-conf` is used to enable any [configuration option](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/#configuration-options) we want on NGINX.

`nginx-ingress-tcp-microk8s-conf` and `nginx-ingress-udp-microk8s-conf` are used to expose services over protocols other than HTTP as described in this other tutorial: [Expose TCP and UDP services with the Kubernetes Nginx Ingress Controller](https://marcolenzo.eu/expose-tcp-and-udp-services-with-the-kubernetes-nginx-ingress-controller/).

## Editing the ConfigMaps

We will now edit one of the ConfigMaps to set the use-forwarded-headers to true.

```shell
microk8s.kubectl -n ingress edit configmaps nginx-load-balancer-microk8s-conf
```

You will be shown the ConfigMap in YAML that you can edit. I commented the lines you need to add.

```yaml
apiVersion: v1
kind: ConfigMap
data: # <--- add this!
  use-forwarded-headers: "true" # <--- add this!
metadata:
  ...
```

## Verify the configuration change was detected

We will now check the logs of the actual Pod running as the ingress controller. In your environment the Pod name will be different, make sure to use the correct one.

```shell
microk8s.kubectl -n ingress get pods
microk8s.kubectl -n ingress logs <your pod name here> | grep reload
```

This is the output we expect.

```text
root@ubuntu:~ microk8s.kubectl -n ingress get pods
NAME                                      READY   STATUS    RESTARTS       AGE
nginx-ingress-microk8s-controller-8c2bd   1/1     Running   2 (3d1h ago)   4d12h

root@ubuntu:~ microk8s.kubectl -n ingress logs <your pod name here> | grep reload
I0327 06:10:55.807716       7 controller.go:155] "Configuration changes detected, backend reload required"
I0327 06:10:55.879639       7 controller.go:172] "Backend successfully reloaded"
I0327 06:10:55.879811       7 event.go:282] Event(v1.ObjectReference{Kind:"Pod", Namespace:"ingress", Name:"nginx-ingress-microk8s-controller-8c2bd", UID:"fbc5a43f-780c-4f9e-bcaf-edbbce4b61a8", APIVersion:"v1", ResourceVersion:"257743", FieldPath:""}): type: 'Normal' reason: 'RELOAD' NGINX reload triggered due to a change in configuration
```

## Done

Done! That was easy!  

![Too Easy](/assets/img/2022/12/too-easy.jpg)
  
Hope you enjoyed it and let me know if you want to tackle some other topic in particular!