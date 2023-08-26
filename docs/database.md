# Database documentation

Databases is important for every webdeveloper, lets set one up in docker to make life easy.

## Requirements

* Docker
* Docker-Compose

## How to install the database docker

1. Create a folder where we store the docker-compose file
   
    `mkdir ~/database`
    
    `cd ~/database`
   
    `touch docker-compose.yml`

2. Open the docker-compose file and put in the following text

    `sudo nano docker-compose.yml`

Dont forget to put in your root password!
```
version: "3"
services:
    database:
        image: mysql
        volumes:
          - db_data:/var/lib/mysql
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: #SETPASSWORDFORROOT
        container_name: database
        ports:
          - "3306:3306"
volumes:
    db_data:
networks:
    default:
        external:
            name: nginx-proxy
```

3. Run the docker-compose script

    `sudo docker-compose up -d`

### PHPMyAdmin

We want to access our database running on a different machine/host

Dont forget to fill in the ip-address of the host machine and the root password!

`sudo docker run --name phpmyadmin -d -e PMA_HOST=xxx.xxx.xxx.xxx -e PMA_USER=root -e PMA_PASSWORD=#SETPASSWORD -p 8081:80 ebspace/armhf-phpmyadmin`

*Note that this docker doesn't run on the reverse proxy, so local network only (that is also for security reasons)*


### extra documentation

Found extra documentation which is unknown if it should be used... one way to find out!

`sudo docker run --name phpmyadmin -d -e PMA_ARBITRARY=1 -p 8081:80 mt08/rpi-phpmyadmin`

on database:

`sudo docker exec -it database sed -i -e 's/# default-authentication-plugin=mysql_native_password/default-authentication-plugin=mysql_native_password/g' /etc/mysql/my.cnf`