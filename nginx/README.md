# NGINX Docker Service for Synology

This project provides a lightweight, ready-to-deploy NGINX container using Docker Compose, designed specifically for Synology NAS via Container Manager.

## Overview

- Lightweight and customizable NGINX setup.
- Static content served from a bind-mounted volume (Synology-compatible).
- Easily configurable via a `.env` file.
- Default port: `8080 → 80`

## Configuration

All environment variables are defined in the `.env` file:

```env
# Project name
COMPOSE_PROJECT_NAME=my-web

# Docker image version
NGINX_VERSION=alpine-slim

# Port exposed on host
NGINX_HTTP_PORT=8080

# Path to the static content directory
NGINX_DATA_VOLUME_PATH=/volume1/docker/web/my-web/volumes/nginx-data
````

## Directory Structure

```
/volume1/docker/web/my-web/
├── docker-compose.yml
├── .env
└── volumes/
    └── nginx-data/
        └── index.html  ← Your static website files
```

## How to Deploy (Synology Container Manager)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the path containing your `docker-compose.yml` and `.env` files.
5. Click **Create** to launch the project.

> ℹ️ Make sure the path defined in `NGINX_DATA_VOLUME_PATH` exists before creating the project.

## Access

Once running, you can access your static site at:
**[http://your-nas-ip:8080](http://your-nas-ip:8080)**

## 📌 Notes

* You can modify the exposed port by changing `NGINX_HTTP_PORT` in the `.env` file.
* The volume uses bind-mounting for data persistence and compatibility with Synology's file system.

---

Happy hosting! 🎉
