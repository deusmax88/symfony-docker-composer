# SYMFONY-DOCKER-COMPOSE

Symfony 3.4 application with development environment powered by Docker Compose.

## Provisioning

In order to work with Docker Compose you have to install:

* [Docker CE](https://www.docker.com/) - At least version 18.06.0
* [Docker-Compose](https://docs.docker.com/compose/)

**Note**: It is recommended to follow [this guide](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user) in order to manage Docker as a non-root user.

In the project's root folder there is the main file where the architecture is described: `docker-compose.yml`. In there you can find all services configured that you'll get. In order to have everything work correctly you have to copy the `.env.dist` file to the `.env`. The latter holds all the environment variables used by docker-compose to run all the containers.

To bring up all the containers, from project's root folder, run:

```
docker-compose up --build
```

**Note**: If you want to run in detached mode just use the `-d` flag.

You can monitor all active containers running:

```
docker-compose ps
```

At this point you have to connect to *php-fpm* container and download all required dependencies needed for the project executing this command:

```
docker-compose exec -u app -w /var/www/html php-fpm bash -c 'composer install'
```

To stop all the containers just run:

```
docker-compose stop
```

This will set up a cluster of containers with all the tools and services needed to develop on this project.

One of the element of the architecture, the `host_manager`, will automatically add in your hosts file as many entries as the number of container defined in `docker-compose.yml` that expose at least one port. Currently these ones are: 

* [www.symfony-docker-dev.com](http://www.symfony-docker-dev.com)
* [phpmyadmin.symfony-docker-dev.com](http://phpmyadmin.symfony-docker-dev.com)

## What you get
In the provided architecture you have following containers with all the described tools within them:

* php-fpm
    * Centos 7.6
    * Php-fpm 7.1
    * Composer 1.8
    * NodeJS 10
    * Yarn 1.13
* www
    * Centos 7.6
    * Apache 2.4
* mysql
    * MySql 5.7
* phpmyadmin
    * PHPMyAdmin Last available version

## Execution Environment
To define the environment you must modify the *PHPFPM_SYMFONY_ENV* variable contained in `.env` file with the environment you want to use.
The console commands run by default with the `dev` environment; to run a specific command with a different environment just add the option `--env` and specify the desired environment e.g.:

```
docker-compose exec -u app -w /var/www/html php-fpm bash -c 'php bin/console command --env prod'
```