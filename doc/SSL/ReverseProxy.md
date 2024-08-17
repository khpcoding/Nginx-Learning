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

This configuration forwards requests from example.com to a server running on localhost:8080.

## SSL Termination

To secure your reverse proxy with SSL, you need to configure NGINX to handle SSL termination. This involves obtaining an SSL certificate and configuring NGINX to use it.

```bash
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
This configuration redirects all HTTP requests to HTTPS and then proxies them to the backend server.

## Load Balancing
NGINX can also be used to load balance traffic across multiple backend servers:

``` bash
http {
    upstream backend {
        server backend1.example.com;
        server backend2.example.com;
    }

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```
This configuration will distribute requests to backend1.example.com and backend2.example.com.

## Testing
To test your reverse proxy setup, restart NGINX and use tools like curl or a web browser to access your domain. Ensure that requests are correctly proxied to the backend server(s).

``` bash
sudo systemctl restart nginx
curl -I http://example.com
```

