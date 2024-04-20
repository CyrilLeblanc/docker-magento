# docker-magento

A Docker stack for Magento **2.4.6-p5** (CE) for development and testing.

## Containers

| Container  | Image                                                                                        | Version | Description         |
| ---------- | -------------------------------------------------------------------------------------------- | ------- | ------------------- |
| PHP        | [php:8.2-apache](https://hub.docker.com/_/php)                                               | 8.2     | Magento source code |
| MariaDB    | [mariadb:10.6](https://hub.docker.com/_/mariadb)                                             | 10.6    | Database            |
| Redis      | [redis:7.0](https://hub.docker.com/_/redis)                                                  | 7.0     | Cache               |
| OpenSearch | [opensearchproject/opensearch:2.12.0](https://hub.docker.com/r/opensearchproject/opensearch) | 2.12.0  | Search Engine       |
| Varnish    | [varnish:7.5](https://hub.docker.com/_/varnish)                                              | 7.5     | HTTP Cache          |
| Nginx      | [nginx:1.24](https://hub.docker.com/_/nginx)                                                 | 1.24    | SSL Termination     |
| phpMyAdmin | [phpmyadmin/phpmyadmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin)                      | latest  | Database GUI        |
| MailHog    | [mailhog/mailhog](https://hub.docker.com/r/mailhog/mailhog)                                  | latest  | Email Testing       |

Request pass through the following order: Nginx -> Varnish -> Apache(PHP)

Nginx handle SSL, Varnish handle HTTP cache, Apache(PHP) handle PHP request

## Requirements

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Installation

1. Clone this repository
2. Copy `.env.sample` to `.env` and update the environment variables
3. Run `docker-compose up -d`