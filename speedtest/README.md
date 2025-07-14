# SpeedTest on Synology with Docker Compose

This project deploys a lightweight speed test server using OpenSpeedTest, designed to run on Synology NAS via Container Manager.

## Overview

- Host your own private internet speed test server.
- Access via HTTP and HTTPS.
- Ideal for testing local network performance.
- Based on the official `openspeedtest/latest` image.

## Configuration

All environment variables are set in the `.env` file:

```env
# Project name
COMPOSE_PROJECT_NAME=network-speedtest

# Docker image version
OPENSPEEDTEST_VERSION=latest

# Timezone for the container
TZ=Europe/Madrid

# Ports for web access
WEB_HTTP_PORT=9004
WEB_HTTPS_PORT=9005
````

## How to Deploy (Synology Container Manager)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the path to your `docker-compose.yml` file (and `.env`, if applicable).
5. Click **Create** to launch the project.

> ℹ️ Ensure that the `.env` file is in the same directory as the `docker-compose.yml` when selecting the folder.

## Access

* HTTP: [http://your-nas-ip:9004](http://your-nas-ip:9004)
* HTTPS: [https://your-nas-ip:9005](https://your-nas-ip:9005)

## Notes

* No persistent volume required.
* You can adjust ports and timezone in the `.env` file before deployment.
* Useful for both LAN and internet speed diagnostics.

---

Enjoy fast and private speed testing! ⚡
