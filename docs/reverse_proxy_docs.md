# Reverse proxy with SSL certs

To manage all websites you need a reverse proxy with automatic SSL certificates.
How it works is pretty simple.

First when you type in the url of the website you want to visit you will be directed to the reverse proxy.
This will first setup a secure connection with the certbot docker that is connected to it.
After that it's going to see if there is a docker container running in the same network with the specified
url configured. If so, it will pass on the secure connection to the docker container hosting the website.
if not it will throw back a error page with not found.

## Requirements

You need the following applications installed on your server:
* Docker
* Docker Compose

## How to setup a NGINX reverse proxy with SSL certificates

1. Create a new docker network for the reverse proxy.

    `sudo docker network create nginx-proxy`

2. Create a folder where we put the docker-compose file
    
    `mkdir -p ~/nginx-proxy`
    
    `cd nginx-proxy`

    `touch docker-compose.yml`

3. Put the following in the docker-compose.yml

```
version: "3"
services:
    nginx-proxy:
        image: jwilder/nginx-proxy
        container_name: nginx-proxy
        restart: always
        ports:
          - "80:80"
          - "443:443"
        volumes:
          - /var/run/docker.sock:/tmp/docker.sock:ro
          - letsencrypt-certs:/etc/nginx/certs
          - letsencrypt-vhost-d:/etc/nginx/vhost.d
          - letsencrypt-html:/usr/share/nginx/html
    letsencrypt-proxy:
        image: jrcs/letsencrypt-nginx-proxy-companion
        container_name: letsencrypt-proxy
        restart: always
        volumes:
          - /var/run/docker.sock:/var/run/docker.sock:ro
          - letsencrypt-certs:/etc/nginx/certs
          - letsencrypt-vhost-d:/etc/nginx/vhost.d
          - letsencrypt-html:/usr/share/nginx/html
        environment:
          - DEFAULT_EMAIL=nicolaydammer@gmail.com
          - NGINX_PROXY_CONTAINER=nginx-proxy
networks:
    default:
        external:
            name: nginx-proxy
volumes:
    letsencrypt-certs:
    letsencrypt-vhost-d:
    letsencrypt-html:
```

4. Start up the docker-compose script and wait for it to run
    
    `sudo docker-compose up -d`

*It might happen that docker screws something up and bugs out. Use the following command:*
`sudo aa-remove-unknown`