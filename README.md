# Nginx-Reverse-Proxy-for-Nexus-Docker-Registries

# Overview

This is a solution of a common problem with Nexus Docker repositories. The administrator has to expose port for "pull", another port for "push", other ports for each hosted repository.
This solution is about leveraging Nginx reverse proxy to avoid using these ports.

# How it works ?
Given :

- Nexus hostname is "nexus.example.com"
- Nexus web port is 8081
- A hosted repository is named "docker-hosted"
- A group repository is named "docker-group"
- Your nginx (with the nginx.conf of this gist) will run for example under cregistry.example.com

The following Nginx configuration file  is for a reverse proxy without the need to expose connector ports from nexus : 

- `docker pull cregistry.example.com/myimage` lets Nginx forward the request to "docker-group"
- `docker push cregistry.example.com/myimage` lets Nginx forward the request to "docker-hosted"

# Notes

- If you have more than one hosted repository, create another Nginx reverse proxy for it, then aggregate them using a parent Nginx reverse proxy that forwards the request according to certain criteria (.i.e: Host header).

- All Nexus repositories must have consistent configuration of authentication: Either all require authentication, or all don't.

- If TLS is enabled with Nexus, change `proxy_set_header X-Forwarded-Proto "http";` by `proxy_set_header X-Forwarded-Proto "https";`

# Source

https://gist.github.com/abdennour/74c5de79e57a47f3351217d674238da8
