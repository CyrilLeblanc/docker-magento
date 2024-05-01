# docker-magento

A docker-compose template for Magento **2.4.6-p5** (CE) for development and testing.

## Containers

| Container  | Image                                                                                        | Version | Description         |
| ---------- | -------------------------------------------------------------------------------------------- | ------- | ------------------- |
| PHP        | [php:8.2-apache-buster](https://hub.docker.com/_/php)                                        | 8.2     | Magento source code |
| MariaDB    | [mariadb:10.6](https://hub.docker.com/_/mariadb)                                             | 10.6    | Database            |
| Redis      | [redis:7.0](https://hub.docker.com/_/redis)                                                  | 7.0     | Cache               |
| OpenSearch | [opensearchproject/opensearch:2.12.0](https://hub.docker.com/r/opensearchproject/opensearch) | 2.12.0  | Search Engine       |
| Varnish    | [varnish:7.5](https://hub.docker.com/_/varnish)                                              | 7.5     | HTTP Cache          |
| Nginx      | [nginx:1.24](https://hub.docker.com/_/nginx)                                                 | 1.24    | SSL Termination     |
| phpMyAdmin | [phpmyadmin/phpmyadmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin)                      | latest  | Database GUI        |
| MailHog    | [mailhog/mailhog](https://hub.docker.com/r/mailhog/mailhog)                                  | latest  | Email Testing       |

Request pass through the following order: Nginx -> Varnish -> Apache(PHP)

Nginx handle SSL, Varnish handle HTTP cache, Apache(PHP) handle PHP request

## Setup

1. Download the repository.
2. Copy `.env.example` to `.env`.
3. Update `.env` file with your configuration.
4. Update your `env.php` file.
5. Create SSL certificates.
6. Start the docker containers.

### 1. Download the repository

You can download or clone the repository using the following command.

```sh
git clone <repository-url>
```

### 2. Copy `.env.example` to `.env`

`cd` into the project directory and copy `.env.example` to `.env`.

```sh
cp .env.example .env
```

### 3. Update `.env` file with your configuration

Adapt the following values to your needs.

#### UID, GID

UID and GID are the user and group id of the user on the host machine. \
You can find the UID and GID of the user using the following command.

```sh
# For UID
id -u

# For GID
id -g
```

#### MAGENTO_SRC

The path to the Magento source code on the host machine. \
You can download the Magento source code from the [official website](https://magento.com/tech-resources/download)
or [GitHub](https://github.com/magento/magento2).

#### MARIADB_ROOT_PASSWORD, MARIADB_DATABASE, MARIADB_USER, MARIADB_PASSWORD

These are the database configuration values. \
You can change the values according to your needs.

### 4. Update your `env.php` file

You need to update the `env.php` file in the `MAGENTO_SRC/app/etc` directory with the following configuration.

```sh
# setup mariadb, opensearch, redis, varnish
bin/magento setup:install \
    --db-host=mariadb \
    --db-name=$MARIADB_DATABASE \
    --db-user=$MARIADB_USER \
    --db-password=$MARIADB_PASSWORD \
    --opensearch-host=opensearch \
    --opensearch-port=9200 \
    --redis-host=redis \
    --redis-port=6379 \
    --cache-backend=redis
```

### 5. Create SSL certificates

To create a self-signed SSL certificate, you can use the following command. \
Replace `magento.localhost` with your domain name.

> **Note:** The only required field is `Common Name`. You can leave the other fields empty.

```sh
docker-compose exec nginx openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/certs/magento.localhost.key -out /etc/nginx/certs/magento.localhost.crt
```

### 6. Start the docker containers

```sh
docker-compose up -d --build
```

## Add a store

1. Add a new store in the Magento admin panel.
2. Add your Nginx configuration in `container-nginx/conf.d/`.
3. Add your SSL certificate in `container-nginx/certs/`.
4. Add your `services > magento > extra_hosts` in the `docker-compose.yml` file.
