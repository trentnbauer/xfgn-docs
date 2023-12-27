---
coverY: 0
---

# Pterodactyl

[Link to App](https://panel.agamersgrind.com)

Pterodactyl® is a free, open-source game server management panel built with PHP, React, and Go. Designed with security in mind, Pterodactyl runs all game servers in isolated Docker containers while exposing a beautiful and intuitive UI to end users.

You can follow my guide on the public documentation site to replicate my set up :)&#x20;

## Flowchart

### Software Level

This flowchart shows how the apps integrate

<img src="../.gitbook/assets/file.excalidraw (6).svg" alt="" class="gitbook-drawing">

### Hardware Level

This flowchart shows what lives where

<img src="../.gitbook/assets/file.excalidraw (5).svg" alt="" class="gitbook-drawing">

## Panel

The Panel is hosted on Cocoa, as a docker container

The Panel stack is made up of 3 containers,

* Redis
* MariaDB
* Panel Application

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
version: '3.8'
services:
  database:
    image: mariadb:10.5
    restart: always
    command: --default-authentication-plugin=mysql_native_password
    volumes:
      - db:/var/lib/mysql
    networks:
      - panel
    environment:
      MYSQL_PASSWORD: $MYSQL_PASS
      MYSQL_ROOT_PASSWORD: $MYSQL_PASS_ROOT
      MYSQL_DATABASE: "panel"
      MYSQL_USER: "pterodactyl"
      
  cache:
    image: redis:alpine3.18
    networks:
      - panel
    restart: always
    volumes:
      - cache:/data
      
  panel:
    image: ghcr.io/pterodactyl/panel:v1.11.3
    restart: always
    networks:
      - panel
    ports:
      - $PORT_HTTP:80
    #dns:
    #  - 1.1.1.1
    links:
      - database
      - cache
    volumes:
      - env:/app/var
      - logs:/app/storage/logs
    environment:
      MAIL_FROM: $MAIL_FROM
      MAIL_DRIVER: "smtp"
      MAIL_HOST: $MAIL_SERVER
      MAIL_PORT: $MAIL_PORT
      MAIL_USERNAME: $MAIL_USERNAME
      MAIL_PASSWORD: $MAIL_PASS
      MAIL_ENCRYPTION: "true"
      APP_URL: $PTERO_PANEL_URL
      APP_TIMEZONE: $TZ
      APP_SERVICE_AUTHOR: $MAIL_FROM
      TRUSTED_PROXIES: "*" 
      DB_PASSWORD: $MYSQL_PASS
      APP_ENV: "production"
      APP_ENVIRONMENT_ONLY: "false"
      CACHE_DRIVER: "redis"
      SESSION_DRIVER: "redis"
      QUEUE_DRIVER: "redis"
      REDIS_HOST: "cache"
      DB_HOST: "database"
      DB_PORT: "3306"
networks:
  panel:

volumes:
  cache:
  db:
  env:
  logs:
```
{% endcode %}

### Redis

| Host Volume        | Container Volume | Purpose                              |
| ------------------ | ---------------- | ------------------------------------ |
| Randomly Generated | /data            | Stores the cached files... I assume? |

### MariaDB

| Host Volume               | Container Volume | Purpose           |
| ------------------------- | ---------------- | ----------------- |
| /srv/pterodactyl/database | /var/lib/mysql   | Database location |

### Panel

| Port | Purpose                |
| ---- | ---------------------- |
| 809  | WebUI HTTP             |
| 4439 | WebUI HTTPS (not used) |

| Host Volume       | Container Volume   | Purpose                          |
| ----------------- | ------------------ | -------------------------------- |
| pteropanel\_var   | /app/var           | Contains configuration .env file |
| pteropanel\_nginx | /etc/ngix/http.d   |                                  |
| pteropanel\_certs | /etc/letsencrypt   |                                  |
| pteropanel\_logs  | /apps/storage/logs |                                  |

| Integration | Purpose                                          |
| ----------- | ------------------------------------------------ |
| Mocha       | Connect to the Wings container to manage servers |

## Wings

[Link to GitHub or Website](https://github.com/pterodactyl/wings)

Wings is Pterodactyl's server control plane, built for the rapidly changing gaming industry and designed to be highly performant and secure. Wings provides an HTTP API allowing you to interface directly with running server instances, fetch server logs, generate backups, and control all aspects of the server lifecycle.

This app is hosted on Mocha as a docker container

This stack is made up of 2 containers,

* Wings
* MariaDB

{% code title="docker-compose.yml" overflow="wrap" %}
```yaml
version: '3.8'
services:
  wings:
    image: ghcr.io/pterodactyl/wings:v1.11.7
    restart: always
    networks:
      - wings
    ports:
      - $PORT_SFTP:2022
      - 443:443
      - 8080:8080
    tty: true
    environment:
      - TZ=$TZ
      - WINGS_UID=988
      - WINGS_GID=988
      - WINGS_USERNAME=pterodactyl
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/lib/docker/containers/:/var/lib/docker/containers/
      - config:/etc/pterodactyl/  #config file from Panel
      - /var/lib/pterodactyl/:/var/lib/pterodactyl/ #game server files
      - logs:/var/log/pterodactyl/
      - /tmp/pterodactyl/:/tmp/pterodactyl/
      - /srv/daemon-data/:/srv/daemon-data/

  db:
    image: mariadb:10.11
    restart: always
    command: --default-authentication-plugin=mysql_native_password
    ports:
      - $PORT_DB:3306
    networks:
      - wings
    volumes:
      - db:/var/lib/mysql
    environment:
      - MYSQL_DATABASE="servers"
      - MYSQL_USER="pterodactyl"
      - MYSQL_PASSWORD=$SQL_PASS
      - MYSQL_ROOT_PASSWORD=$SQL_PASS_ROOT

networks:
  wings:
    name: wings
    driver: bridge
    driver_opts:
      com.docker.network.bridge.name: wings


volumes:
  config: 
  lib:
  logs:
  daemon:
  db:
```
{% endcode %}

### Wings

| Port | Purpose               |
| ---- | --------------------- |
| 443  | Panel Connection Port |
| 2022 | SFTP                  |
| 8080 | Daemon port (unused)  |

| Host Volume                 | Container Volume           | Purpose                             |
| --------------------------- | -------------------------- | ----------------------------------- |
| /var/run/docker.sock        | /var/run/docker.sock       | Manage Docker containers ( servers) |
| /var/lib/docker/containers/ | var/lib/docker/containers/ |                                     |
| etc                         | /etc/pterodactyl/          | Config file lives here              |
| /var/lib/pterodactyl/       | /var/lib/pterodactyl/      |                                     |
| /var/log/pterodactyl/       | /var/log/pterodactyl/      |                                     |
| /tmp/pterodactyl/           | /tmp/pterodactyl/          |                                     |
| /srv/daemon-data/           | /srv/daemon-data/          |                                     |

| Integration | Purpose                      |
| ----------- | ---------------------------- |
| Panel       | Panel manages wings instance |

### **MariaDB**

| Port | Purpose                 |
| ---- | ----------------------- |
| 3306 | Port to access Database |

| Host Volume | Container Volume | Purpose           |
| ----------- | ---------------- | ----------------- |
| db          | /var/lib/mysql   | Database location |

| Integration | Purpose                |
| ----------- | ---------------------- |
| Panel       | Panel manages database |
