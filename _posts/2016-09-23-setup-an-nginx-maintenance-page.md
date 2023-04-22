---
title: 'Setup an Nginx Maintenance Page'
date: '2016-09-23'
author: marcolenzo
layout: post
permalink: /setup-an-nginx-maintenance-page/
categories:
    - 'DevOps'
tags:
    - CentOS
    - Nginx
---

Nginx grows in popularity day by day. Its solid performance and ease of setup make it a perfect tool to proxy traffic to your backend. In this article I will describe how you can configure a maintenance page that will be shown automatically every time you re-deploy or possibly your backend goes belly up.

If you already have an Nginx installation you can jump straight here. Otherwise I will guide you into installing Nginx as well. As usual, I will describe the installation procedure for CentOS 7. You can use the same procedure for any Fedora base distro.

## Install Nginx

```shell
yum -y install nginx
```

## Recommended Folder Structure for Configuration Files

```shell
mkdir /etc/nginx/sites-available
mkdir /etc/nginx/sites-enabled
```

We create configuration files in the `sites-available` folder. We activate configurations by symlinking them in the `sites-enabled` folder.

```shell
vi /etc/nginx/sites-available/mydomain-com.conf
ln -s /etc/nginx/sites-available/mydomain-com.conf /etc/nginx/sites-enabled/mydomain-com.conf
```

I will describe the content of the `mydomain-com.conf` later on. For now we just add the following line in `/etc/nginx/nginx.conf` to instruct it to scan our `site-enabled` folder when launching up.

```shell
include /etc/nginx/sites-enabled/*.conf;
```

## Proxy Pass and Maintenance Page

This is the content of our site specific configuration file.

```nginx
server {
    listen       80;
    server_name  your-domain.com;

    # Proxy all requests to your backend
    location / {
        proxy_pass http://127.0.0.1:30080; 
    }

    # Serve static files for http://your-domain.com/offline/* requests
    location /offline/ { 
        root /var/www;
    }

    # redirect 404 server error pages to the static page /40x.html
    error_page  404              /404.html;
    location = /404.html {
        root   /var/www/errors;
    }

    # redirect 500 server error pages to the static page /50x.html
    error_page   500              /50x.html;
    location = /50x.html {
        root   /var/www/errors;
    }

    # Maintenance page. 
    # If your backend is not reached by proxy_pass 
    # the offline.html file you stored in /var/www/offline/ will be shown instead.
    error_page   501 502 503 504  /offline.html;
}
```

As you can see the trick is simple. Proxy all requests to your backend except those hitting the `/offline` URL.

When your backend is down, Nginx will receive a `503 Service Unavailable` error for which you instruct Nginx to display the `offline.html` file stored in `/var/www/offline`.

That’s all folks!