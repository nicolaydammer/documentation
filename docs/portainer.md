# Portainer for easy docker management

Portainer is used for easy access to docker containers and a quick way to manage them.

## Requirements

* Docker

## How to install portainer on the first host

1. Run the following docker command and go the page on port 9000 to setup an account
    
    `sudo docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest`

## How to install portainer on other hosts and connect them

1. Run this command on other hosts and configure them via port 9001

    `sudo docker run -d -p 9001:9001 --name portainer_agent --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/volumes:/var/lib/docker/volumes cr.portainer.io/portainer/agent:2.9.3`