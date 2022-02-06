# Environment for development in Laravel 8 and 9

For Laravel you must change the php version found in the folder: 

```bash
.docker> php-fpm-alpine > Dockerfile
```
Deve ser alterado apenas o  
```bash
ARG PHP_VERSION=8.1.1 
```
for 
```bash
ARG PHP_VERSION=8.0
```
## Dependencies:
  * Docker. See [https://docs.docker.com/engine/installation](https://docs.docker.com/engine/installation)
  * Docker compose. See [docs.docker.com/compose/install](https://docs.docker.com/compose/install/)


## Installing a new laravel project or cloning a project

Use laravel github and select the latest version.

```bash
git clone git@github.com:laravel/laravel.git src
```

## How to run
Climbing the container
```bash
docker-compose up -d
```

## Application Settings
In your terminal type the following commands
```bash
1: docker-compose exec php-fpm cp .env.example .env
2: docker-compose exec php-fpm composer install
3: docker-compose exec php-fpm chown -R www-data:www-data /application/storage
4: docker-compose exec php-fpm php artisan key:generate
```

## Database settings
No docker-compose.yml file, in the mariadb service is the user, password and bank name settings. Copy the information to the .env.
Access phpmyadmin in **host:** `localhost`; **port:** `31003` and create the table as the name is in docker-compose.yml.

#### PHPmyadmin
```bash
HOST=mariadb
DATABASE=MYSQL_DATABASE
USERNAME=root
PASSWORD=MYSQL_ROOT_PASSWORD
```
#### .env
```bash
DB_HOST=mariadb
DB_PORT=3306
DB_DATABASE=MYSQL_DATABASE
DB_USERNAME=root
DB_PASSWORD=MYSQL_ROOT_PASSWORD
```

## Database migrations
```bash
docker-compose exec php-fpm php artisan migrate
```

## .Env permissions
If you have problems saving the .env information, follow the steps below:
```bash
1: docker-compose exec php-fpm sh
2: chmod -R 777 .env
3: exit
```

## Services exposed outside your environment ##

You can access your application via **`localhost`**. Mailhog and nginx both respond to any hostname, in case you want to add your own hostname on your `/etc/hosts`

Service|Address outside containers
-------|--------------------------
Webserver|[localhost:31000](http://localhost:31000)
MariaDB|**host:** `localhost`; **port:** `31003`

## Hosts within your environment ##

You'll need to configure your application to use any services you enabled:

Service|Hostname|Port number
------|---------|-----------
php-fpm|php-fpm|9000
MariaDB|mariadb|3306 (default)
Redis|redis|6379 (default)

# Docker compose cheatsheet #

**Note:** you need to cd first to where your docker-compose.yml file lives.

  * Start containers in the background: `docker-compose up -d`
  * Start containers on the foreground: `docker-compose up`. You will see a stream of logs for every container running. ctrl+c stops containers.
  * Stop containers: `docker-compose stop`
  * Kill containers: `docker-compose kill`
  * View container logs: `docker-compose logs` for all containers or `docker-compose logs SERVICE_NAME` for the logs of all containers in `SERVICE_NAME`.
  * Execute command inside of container: `docker-compose exec SERVICE_NAME COMMAND` where `COMMAND` is whatever you want to run. Examples:
    * Shell into the PHP container, `docker-compose exec php-fpm bash`
    * Run symfony console, `docker-compose exec php-fpm bin/console`
    * Open a mysql shell, `docker-compose exec mysql mysql -uroot -pCHOSEN_ROOT_PASSWORD`
## License
[MIT](https://github.com/josueeek/docker-workspace-laravel-8-and-9/blob/main/LICENSE)
