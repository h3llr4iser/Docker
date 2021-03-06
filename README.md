# Description
This Docker Compose development environment includes

* PHP 7.0
* MariaDB
* Nginx 
* Composer

# Usage

1. Copy .env.dist to .env, and specify you sylius installaion
2. Configure database as following, be careful the host and password
```
parameters:
    database_host: mysql
    database_name: sylius
    database_user: sylius
    database_password: sylius
```
3. You need to install Docker and Docker Compose.
```bash
cd docker
docker-compose up
```

4. login into php container and install sylius.
```
docker exec -it $(docker-compose ps -q php) bash
bin/console sylius:install
```

5. Access your website.
```
http://localhost:8080
```

Now you have a few options to get started

# Todo

1. [-] install gulp

## Basic

Get the ip of the Nginx container.

```
docker inspect $(docker-compose ps -q nginx) | grep IPAddress
```

## Advanced

Run a `dnsdock` container before `docker-compose up`, more info: https://github.com/tonistiigi/dnsdock
Access the containers from the dns records.

## Troubleshooting

## How to enter a container?

Enter the php container to install composer vendors etc.

```bash
docker exec -it $(docker-compose ps -q php) bash
```

## The application is too slow.

Install composer vendors in the container and symlink them to the application directory.
Execute inside the php container:

```bash
mkdir /vendor && ln -sf /vendor ./vendor
```

Using Symfony2 inside Vagrant can be slow due to synchronisation delay incurred by NFS.
You can write the app logs and cache to a folder on the php container.

Enter the php container and create the directory:

```bash
docker exec -it $(docker-compose ps -q php) bash
mkdir /dev/shm/sylius/
setfacl -R -m u:"www-data":rwX -m u:`whoami`:rwX /dev/shm/sylius/
setfacl -dR -m u:"www-data":rwX -m u:`whoami`:rwX /dev/shm/sylius/
```

To view the application logs, run the following commands:

```bash
tail -f /dev/shm/sylius/app/logs/prod.log
tail -f
```


