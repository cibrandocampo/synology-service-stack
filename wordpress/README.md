# WordPress + MariaDB Docker Service for Synology

This project provides a complete WordPress stack with MariaDB, ready to deploy on Synology NAS via Container Manager using Docker Compose.

## Overview

- WordPress with persistent data and database volumes.
- Easily configurable via `.env` file.
- Designed for Synology NAS with bind-mounted volumes.
- Default HTTP port: `9210 → 80`

## Configuration

All environment variables are defined in the `.env` file:

```env
# Project
COMPOSE_PROJECT_NAME=my-wordpress-web

# Docker versions
MARIADB_VERSION=lts
WORDPRESS_VERSION=6

# Database settings
MYSQL_DATABASE=wp-my-web-database
MYSQL_ROOT_PASSWORD=change-me
MYSQL_USER=wp-my-web
MYSQL_PASSWORD=change-me

# Port mapping
WORDPRESS_HTTP_PORT=9210
TZ=Europe/Madrid

# Volume paths (adjust to your folder structure)
WORDPRESS_DB_VOLUME_PATH=/volume1/docker/web/my-wordpress-web/volumes/mariadb
WORDPRESS_DATA_VOLUME_PATH=/volume1/docker/web/my-wordpress-web/volumes/wordpress
````

## Directory Structure

```
/volume1/docker/web/my-wordpress-web/
├── docker-compose.yml
├── .env
└── volumes/
    ├── mariadb/        ← Persistent database data
    └── wordpress/      ← WordPress wp-content directory
```

## How to Deploy (Synology Container Manager)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the path containing your `docker-compose.yml` and `.env` files.
5. Click **Create** to launch the project.

> ⚠️ Make sure the folders defined in `WORDPRESS_DB_VOLUME_PATH` and `WORDPRESS_DATA_VOLUME_PATH` exist before deployment.

## Access

Once deployed, access your WordPress site at:
**[http://your-nas-ip:9210](http://your-nas-ip:9210)**

## 📌 Notes

* Change the database passwords before deploying (`MYSQL_ROOT_PASSWORD` and `MYSQL_PASSWORD`).
* You can modify the port by updating `WORDPRESS_HTTP_PORT` in the `.env` file.
* The MariaDB container stores data in a persistent bind-mounted volume.
* The WordPress container persists content from the `wp-content` directory.

---

Create and manage your own website, blog, or project with ease! 📝🌐
