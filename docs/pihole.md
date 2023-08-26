# Pihole for all your adblock and DNS needs

Pihole is usefull for adblocking your entire network if used as your dns. 
Which is also where it comes in that its a good DNS to use in your network.

## Requirements

* Docker
* Docker-Compose

## Installing pihole docker

1. Create folder with the docker-compose file
    
    `mkdir ~/pihole`

    `cd pihole`

    `touch docker-compose.yml`

2. Put the following in the docker-compose file

    `sudo nano docker-compose.yml`

```
version: "2"
services:
    pihole:
        container_name: pihole
        image: pihole/pihole:v5.1.2
        restart: always
        environment:
          - TZ=Europe/Amsterdam
          - WEBPASSWORD=TeringHomo1997!
          - VIRTUAL_HOST=pihole.pi.local
          - LETSENCRYPT_HOST=pihole.pi.home
        network_mode: host
        volumes:
          - './etc-pihole/:/etc/pihole/'
          - './etc-dnsmasq.d/:/etc/dnsmasq.d/'
        cap_add:
          - NET_ADMIN
        expose:
          - 80
          - 443
networks:
    default:
        external:
            name: nginx-proxy
```

3. Run the docker-compose script

    `sudo docker-compose up -d`