---
title: 'Install WordPress on Kubernetes'
date: '2019-12-02'
author: marcolenzo
layout: post
permalink: /install-wordpress-on-kubernetes/
categories:
    - 'DevOps'
tags:
    - Kubernetes
    - Wordpress
---

WordPress is without doubt the leading content management tool on the internet. It’s simple to use and it has an endless set of features and plugins. However, its setup and upgrade are not always straightforward. Today, I will teach you how to run it in just few minutes on a Kubernetes cluster. I will also show how to tweak its configuration and deal with backups.

## The full manifest

If you just want to get straight to the point, copy the Kubernetes manifest below, tweak it and apply it to your cluster. Otherwise, continue reading and I will explain each part into more detail.

```yaml
apiVersion: v1
kind: Service
metadata: 
  name: wordpress
spec:
  selector:
    app: wordpress
  ports:
  - port: 80
    targetPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      volumes:
        - name: wordpress
          hostPath:
            path: /srv/k8s-volumes/wordpress
      containers:
      - name: wordpress
        image: wordpress:5.3.0
        ports:
        - containerPort: 80
        env:
        - name: WORDPRESS_DB_HOST
          value: mysql:3306
        - name: WORDPRESS_DB_USER
          value: wordpress
        - name: WORDPRESS_DB_PASSWORD
          value: password
        - name: WORDPRESS_DB_NAME
          value: wordpress
        - name: WORDPRESS_CONFIG_EXTRA
          value: >
            define('FS_METHOD','direct');
        volumeMounts:
          - name: wordpress
            mountPath: /var/www/html/wp-content
---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: wordpress
spec:
  rules:
  - host: marcolenzo.eu
    http:
      paths:
      - backend:
          serviceName: wordpress
          servicePort: 80
```

### The Service resource

```yaml
apiVersion: v1
kind: Service
metadata: 
  name: wordpress
spec:
  selector:
    app: wordpress
  ports:
  - port: 80
    targetPort: 80
```

A [Kubernetes Service](https://kubernetes.io/docs/concepts/services-networking/service/) is a logical abstraction to expose an application running on a set of Pods. In this case, we are creating a service named `wordpress `that will be reachable at <http://wordpress> within the cluster.

### The Deployment resource

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      volumes:
        - name: wordpress
          hostPath:
            path: /srv/k8s-volumes/wordpress
      containers:
      - name: wordpress
        image: wordpress:5.3.0
        ports:
        - containerPort: 80
        env:
        - name: WORDPRESS_DB_HOST
          value: mysql:3306
        - name: WORDPRESS_DB_USER
          value: wordpress
        - name: WORDPRESS_DB_PASSWORD
          value: password
        - name: WORDPRESS_DB_NAME
          value: wordpress
        - name: WORDPRESS_CONFIG_EXTRA
          value: >
            define('FS_METHOD','direct');
        volumeMounts:
          - name: wordpress
            mountPath: /var/www/html/wp-content
```

A [Deployment ](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)is used to define the desired state of your application. Kubernetes will take care of applying the state and self-heal it if it notices any issue with it.

As `image`, we are using the `wordpress` official distribution available at [https://hub.docker.com/\_/wordpress](https://hub.docker.com/_/wordpress). You can find more information about it and which versions are available by visiting its website.

As you know, **WordPress requires a MySQL database** to be able to store part of its data. We configure the database details by setting specific environment variables such as `WORDPRESS_DB_HOST`, `WORDPRESS_DB_USER`, `WORDPRESS_DB_PASSWORD` and `WORDPRESS_DB_NAME`. You could theoretically run MySQL on Kubernetes as well or leverage any other installation available to you.

Since the `wp-config.php` ships with the container image, we need a way to specify the additional configuration lines we would include there. Rather than building our own image injecting a custom `wp-config.php`, we can use another environment variable named `WORDPRESS_CONFIG_EXTRA`. In this particular example, I am instructing WordPress to use the filesystem directly with the `define('FS_METHOD','direct');` instruction. This is necessary to avoid those annoying FTP popups whenever we try to fiddle with plugins and themes.

Finally, we need to cater for the storage of themes, plugins, images and other large files that WordPress stores on disk rather than the database. For this task, we use [Kubernetes Volumes](https://kubernetes.io/docs/concepts/storage/volumes/). In this particular example, I am using a `hostPath`, i.e. I am storing the data on the same machine where the Pod is spawn. Be aware that **this configuration would not work in clusters with multiple nodes** as your Pod could be spawned in different nodes. If you are using Kubernetes as a service, the easiest option should be to choose a volume supported by your cloud provider such as the [`gcePersistentDisk` ](https://kubernetes.io/docs/concepts/storage/volumes/#gcepersistentdisk)or `<a href="https://kubernetes.io/docs/concepts/storage/volumes/#awselasticblockstore">awsElasticBlockStore</a>`.

Finally, we mount the volume we defined in the container at `/var/www/html/wp-content` using the `volumeMounts` element.

### The Ingress resource

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: wordpress
spec:
  rules:
  - host: marcolenzo.eu
    http:
      paths:
      - backend:
          serviceName: wordpress
          servicePort: 80
```

A [Kubernetes Service](https://kubernetes.io/docs/concepts/services-networking/service/) is visible only within the cluster. We need a way to expose such service outside the cluster. There are multiple ways to do so. The one I am proposing is to use a [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/). Note that you need to have an ingress controller pre-installed in the cluster. If you don’t have one, follow the installation guide for the [Nginx Ingress Controller](https://kubernetes.github.io/ingress-nginx/).

Make sure to replace the `host `with your personal domain. The rest can be left as is.

Just head over to your URL and if you have everything setup correctly you should be able to see WordPress up and running. **If you experience issues with redirects** being sent to the wrong domain or protocol, i.e. HTTP instead of HTTPs, have a look at this article: [Enable X-Forwarded-\* Headers in Kubernetes Nginx Ingress Controller](https://marcolenzo.eu/2019/12/01/enable-x-forwarded-headers-in-kubernetes-nginx-ingress-controller/).

## Backups

What we need to backup is simply the data wordpress stores on the database and the volume we mount to it. We do not need to backup anything else. While there are many ways you can achieve that by using some automation tool or cron bash scripts, I advice you to have a look at the [UpdraftPlus plugin](https://wordpress.org/plugins/updraftplus/). It will make your life much easier. If you want to do it your own way, the backup strategy is specific to the volume and database you are using.

## Conclusion

We used one single manifest file to spawn a WordPress installation in seconds. We could create as many as we want just by creating new manifests, while being careful to point to distinct database schemas and volumes. You might need to tweak the configuration to make it work in your particular setup, but it should be enough to give you a head-start.

Enjoy!