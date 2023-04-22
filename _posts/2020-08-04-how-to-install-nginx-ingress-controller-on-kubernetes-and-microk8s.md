---
title: 'Install Nginx Ingress Controller on Kubernetes and MicroK8s'
date: '2020-08-04'
author: marcolenzo
layout: post
permalink: /how-to-install-nginx-ingress-controller-on-kubernetes-and-microk8s/
categories:
    - 'DevOps'
tags:
    - Kubernetes
    - Kubernetes Ingress
    - MicroK8s
    - Nginx
---

Nginx is the most popular Ingress Controller with Kubernetes. We find it in many Kubernetes distributions enabled by default. In this post, I will show you install and configure the Nginx Ingress Controller in few steps.

## Existent Installation Check

Before going ahead and installing the Nginx Ingress Controller, we verify if it is already running in your Kubernetes cluster. Above all, we want to avoid the risk of creating a mess in your setup by installing it twice.

All you need to do is to look for the existence of `nginx-ingress-controller` Pods in your system

```shell
kubectl get pods --all-namespaces | grep nginx-ingress
```

```text
marcol@ubuntu:~$ kubectl get pods --all-namespaces | grep nginx-ingress
nginx-ingress-controller-5bbd46cd86-l8sxr   1/1     Running   0          32d
```

If your output is similar to the above, congratulations! You already have an ingress controller! 🎉

## Installing Nginx Ingress Controller on MicroK8s

In MicroK8s, we simply enable the `ingress` addon to install the Nginx Ingress Controller.

```shell
microk8s enable ingress
```

If you don’t know what MicroK8s is have a look at my tutorial [here](https://marcolenzo.eu/2019/12/14/easiest-kubernetes-install-ever-certified-kubernetes-with-just-one-command-line/). MicroK8s is a lightweight Kubernetes distribution supported by Canonical which is super easy to install and maintain. It is perfect for IOT, developer machines and your Raspberry Pi. If you don’t have MicroK8S at all you can follow my video tutorial to get up and running in just 10 minutes.

{% include embed/youtube.html id='3T6skoL3RTA' %}

## Installing Nginx Ingress Controller on Kubernetes (any distribution)

Simply execute the manifest provided within the Kubernetes GitHub repository:

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml
```

The deployment was successful if the relevant Pod is in `Running` state.

```text
marcol@ubuntu:~$ kubectl get pods -n ingress-nginx
NAME                                        READY   STATUS    RESTARTS   AGE
nginx-ingress-controller-5bbd46cd86-l8sxr   1/1     Running   0          4m
```

**Now that the Pod is deployed, we need to choose how traffic is directed to it.** There are several options depending on how you created your cluster and where it resides. For the purpose of this tutorial, I will give you two options: cloud and custom.

### Cloud Environments (GCP, AWS, Azure, etc…)

In cloud environments that support the `LoadBalancer` service type you can run the following:

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/cloud-generic.yaml
```

Wait for the configuration to be applied and jot down the service external IP address once available:

```text
marcol@ubuntu:~$ kubectl get service -n ingress-nginx
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)        AGE
ingress-nginx          LoadBalancer   192.0.2.167     1.1.1.1          [...]          4m
```
```

To verify that everything is setup correctly, execute an HTTP request to any path you wish. If you get a 404 reply back, your Ingress controller is correctly configured. Replace `1.1.1.1` which the actual IP of your load balancer.

```shell
marcol@ubuntu:~$ curl https://1.1.1.1/whatever
```

```html
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>openresty/1.15.8.2</center>
</body>
</html>
```

### Custom environments 

In other type of environments, such your own machine, we can expose the Ingress Controller on port 80 and port 443 by attaching the Pod to the host network. Let’s execute an `edit` on the `nginx-ingress-controller` deployment.

```shell
kubectl edit deployment nginx-ingress-controller -n ingress-nginx
```

We now add the `hostNetwork: true` clause under the `template.spec` section.

```yaml
apiVersion: apps/v1
kind: Deployment
[...]
spec:
  [...]
  template:
    hostNetwork: true
```

To verify that everything is setup correctly, execute an HTTP request to any path you wish. If you get a 404 reply back, your Ingress controller is correctly configured. Set `NODE_IP` to the IP of one of your nodes.

```shell
marcol@ubuntu:~$ curl https://${NODE_IP}/whatever
```

```html
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>openresty/1.15.8.2</center>
</body>
</html>
```

**If you do not wish to attach the Nginx Ingress Controller Pod to the host network**, your only option is to expose it on higher port by changing the `ingress-nginx` service type from `LoadBalancer` to `NodePort`. You can find more information on the [official documentation](https://kubernetes.github.io/ingress-nginx/deploy/baremetal/).

## Conclusion

In this post, we installed the Nginx Ingress Controller and verified our installation. Now you can make use of the [Ingress resource](https://kubernetes.io/docs/concepts/services-networking/ingress/) to direct traffic in your Kubernetes cluster.