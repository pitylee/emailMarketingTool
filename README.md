# Laravel tool

## For the Laravel Tool
Go to the Laravel readme found in [its corresponding readme](./tool/readme.md)

# Laradock 
## Initialize the repository

You can download the repo as a zip, or **clone** on your workspace if you have the proper git setup:

```
git clone git@github.com:pitylee/emailMarketingTool.git
```

If you are not already in `laradock` directory, first change the current dir where you have the project:

```
cd /path/to/project/dir/
```

And then pull the laradock submodule, in some cases with older version of git this is needed:

```
git submodule update --init --recursive
```

## Dependencies

While a stable internet connection is needed, you may have to install **Docker (Desktop)** and **Docker Compose** if you
don't have already installed. If you still have to install, please follow the official docs to do
so [here](https://docs.docker.com/desktop/#download-and-install) and [here](https://docs.docker.com/compose/install/).

## Before we begin

### Give exec permission to laradock

To give executive permission, run the following command:

```
chmod +x ./.start
```

### Copy conf and env files

You have to copy the nginx default.conf file from the root, to the laradock submodule's nginx sites directory:

```
cp default.conf ./laradock/nginx/sites/default.conf
```

You have to copy the corresponding .env file from the root, to the laradock submodule's directory:

```
cp .env.laradock ./laradock/.env
```

You have to copy the corresponding .env file from the root, to the tool directory for Laravel:

```
cp .env.laravel ./tool/.env
```

## Usage

### If you have already ran ./start or the docker-compose commands

At this point, if you have already ran Laradock, you have to restart your Docker Desktop, and re-build the images, more
specifically the nginx one, in order to force load the nginx configuration.

### Start docker environment

For short usage on Mac and Linux, you can run via the file which contains the commands:

```
    bash .start 
```

This will run the `docker-compose` commands, or you can rule them manually, via running in the terminal of your
preference:

```
cd ./laradock

docker-compose up -d --build nginx mysql redis php-worker

docker-compose exec --user=laradock workspace bash
```

## In the Laradock environment

When you see the that the Docker containers are up and running, and entered the workspace with one of the two solutions
from above, you want to do a few small things:

### First you want to navigate to `/var/www/tool`:

```
cd tool/
```

### Install composer packages for Laravel

```
composer install 
```

### Generate a secure key for Laravel

```
php artisan key:generate
```

### Mysql password check and migration

By default, the username and password for Laradock Mysql will be root:root.
_This is included in the .env, but you may want to double check with DBeaver, your Database editor of your <3's choice._

The migration:

```
php artisan migrate
```

The seeds:

```
php artisan db:seed ...
```


The tests:

```
php artisan test
```

## _Handy_

* It can come in handy, that we have logs about the incoming requests + the generated MAC can be seen there as well.
GUI for this:
[http://localhost/log-viewer/](http://localhost/log-viewer/)

## _Troubleshoot_

* If you have a 500 error, in the laradock/ folder:

```
docker-compose logs nginx
```

* For when Docker fails to run LLB, run in your terminal, or edit+source your .(z)shrc the following:

```
export DOCKER_BUILDKIT=0
export COMPOSE_DOCKER_CLI_BUILD=0
```
