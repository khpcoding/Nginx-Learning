# NGINX Reverse Proxy Setup

This repository provides a simple guide to setting up a reverse proxy using NGINX. A reverse proxy is a server that sits in front of web servers and forwards client requests to those web servers. NGINX is a powerful tool that can serve as a reverse proxy, load balancer, and more.

## Table of Contents

- [What is a Reverse Proxy?](#what-is-a-reverse-proxy)
- [Why Use NGINX for Reverse Proxy?](#why-use-nginx-for-reverse-proxy)
- [Installation](#installation)
- [Configuration](#configuration)
  - [Basic Reverse Proxy Setup](#basic-reverse-proxy-setup)
  - [SSL Termination](#ssl-termination)
  - [Load Balancing](#load-balancing)
- [Testing](#testing)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## What is a Reverse Proxy?

A reverse proxy is a type of proxy server that retrieves resources on behalf of a client from one or more servers. These resources are then returned to the client as if they originated from the reverse proxy itself. 

## Why Use NGINX for Reverse Proxy?

NGINX is widely used for its high performance, stability, rich feature set, simple configuration, and low resource consumption. It is capable of handling a large number of simultaneous connections, which makes it ideal for use as a reverse proxy.

## Example 
Here is an example configuration:

``` bash
server {
    listen 80;

    server_name example.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
