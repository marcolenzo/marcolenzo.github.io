---
title: 'Easiest Kubernetes Install ever! Certified Kubernetes with just one command line'
date: '2019-12-14'
author: marcolenzo
layout: post
permalink: /easiest-kubernetes-install-ever-certified-kubernetes-with-just-one-command-line/
categories:
    - 'DevOps'
tags:
    - Kubernetes
    - MicroK8s
---

Kubernetes has a fairly complex architecture. For years, developers and operators shied away from trying it out because of its [complex installation](https://marcolenzo.eu/2016/04/14/create-a-kubernetes-cluster-on-centos-7/). Its popularity pushed the community to find ways to simplify this process and tools like [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) were created. However, even `kubeadm` is not straightforward for most. Today I will show you the simplest way I found out to have a certified Kubernetes installation in just one command line: [MicroK8s](https://microk8s.io/).

## Install

As promised the install requires only one command line on systems that supports snaps such as Ubuntu.

```shell
sudo snap install microk8s --classic
```

Done! Use `microk8s.kubectl` to manage your cluster.

```shell
microk8s.kubectl get nodes
```

```
NAME            STATUS   ROLES    AGE     VERSION
ubuntu-server   Ready    <none>   1m      v1.17.0
```

## Addons

MicroK8s comes prepackaged with some popular addons like Istio, DNS, and much more. List them with `microk8s.status` and enable them with `microk8s.enable`. I advise you to install the `dns` addon at least.

```shell
microk8s.enable dns
```

```shell
microk8s.status
```

```
microk8s is running
addons:
cilium: disabled
dashboard: disabled
dns: enabled
fluentd: disabled
gpu: disabled
helm: disabled
ingress: disabled
istio: disabled
jaeger: disabled
juju: disabled
knative: disabled
kubeflow: disabled
linkerd: disabled
metallb: disabled
metrics-server: disabled
prometheus: disabled
rbac: disabled
registry: disabled
storage: disabled
```

## Adding more nodes

You can easily add nodes with MicroK8s in just two steps.

Run `microk8s.add-node` on the node you wish to act as master. This command will install the control plane and output the command you need to run on the other nodes to join them.

```shell
microk8s.add-node
```
```
Join node with: microk8s.join 192.168.202.85:25000/GUvBqpyxYFcOANDKRmwxmEpfRtaTQmRm

If the node you are adding is not reachable through the default interface you can use one of the following:
 microk8s.join 192.168.202.85:25000/GUvBqpyxYFcOANDKRmwxmEpfRtaTQmRm
 microk8s.join 10.1.76.0:25000/GUvBqpyxYFcOANDKRmwxmEpfRtaTQmRm
 microk8s.join 172.17.0.1:25000/GUvBqpyxYFcOANDKRmwxmEpfRtaTQmRm
```

Then, access the other nodes and run the outputted command. *You need to have MicroK8s pre-installed on the node.*

```shell
microk8s.join 192.168.202.85:25000/GUvBqpyxYFcOANDKRmwxmEpfRtaTQmRm
```

Done. Let’s go back to the master and check if the node is visible.

```shell
microk8s.kubectl get nodes
```

```
NAME                   STATUS   ROLES    AGE     VERSION
ubuntu-server          Ready    <none>   20m     v1.17.0
ubuntu-server-node01   Ready    <none>   11s     v1.17.0
```

## Testing the installation

Let’s create a Pod with a `busybox` container. We will then execute a shell on it and test out DNS resolution.

```shell
microk8s.kubectl apply -f https://raw.githubusercontent.com/marcolenzo/blogfiles/master/kubernetes/pods/busybox.yaml
```

Let’s verify that the Pod is running.

```shell
microk8s.kubectl get pods
```

```
NAME      READY   STATUS    RESTARTS   AGE
busybox   1/1     Running   0          2m5s
```

Let’s get a shell and try to resolve the `kubernetes` service through DNS.

```shell
microk8s.kubectl exec -ti busybox sh
```

```
/ # nslookup kubernetes
Server:         10.152.183.10
Address:        10.152.183.10:53

Name:   kubernetes.default.svc.cluster.local
Address: 10.152.183.1
```

Perfect!

![Spongebob](/assets/img/spongebob.jpg)

## Conclusion

You have no excuses! You have to try out Kubernetes. It has been democratized up to the level that it takes a single command line to have a certified installation running wherever you want: Linux, Windows and macOS. Start using it and you will never go back to traditional deployments.