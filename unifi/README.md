# UniFi Controller + MongoDB for Synology

This project provides a ready-to-deploy UniFi Controller stack with MongoDB, designed to run on Synology NAS via Container Manager using Docker Compose.

## Overview

- Run your own UniFi Controller to manage UniFi network devices.
- Includes MongoDB as required by the UniFi backend.
- Uses bind-mounted volumes for persistence and compatibility with Synology's file system.
- Exposes required TCP/UDP ports for device communication and discovery.

## Configuration

All environment variables are defined in the `.env` file:

```env
# Project
COMPOSE_PROJECT_NAME=network-infra

# Docker image versions
MONGO_VERSION=4.4
UNIFI_VERSION=v10

# UniFi config
SYSTEM_IP=192.168.1.x
UNIFI_HTTP_PORT=9001
UNIFI_HTTPS_PORT=9002
TZ=Europe/Madrid

# Volume paths
UNIFI_DB_DATA_VOLUME_PATH=/volume1/docker/network/infra/volumes/unifi-db-data
UNIFI_DB_CONFIG_VOLUME_PATH=/volume1/docker/network/infra/volumes/unifi-db-config
UNIFI_VOLUME_PATH=/volume1/docker/network/infra/volumes/unifi-data
```

> ⚠️ Note: MongoDB 5.0+ requires a CPU with AVX support. Use version 4.4 if your Synology does not support AVX.

## Directory Structure

```
/volume1/docker/network/infra/
├── docker-compose.yml
├── .env
└── volumes/
    ├── unifi-db-data/       ← MongoDB database data
    ├── unifi-db-config/     ← MongoDB config
    └── unifi-data/          ← UniFi Controller data
```

## How to Deploy (Synology Container Manager)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the path containing your `docker-compose.yml` and `.env` files.
5. Click **Create** to launch the project.

> ℹ️ Ensure that all volume paths exist before launching the stack, or Synology may fail to bind them.

## Access

* Web interface (HTTP): [http://your-nas-ip:9001](http://your-nas-ip:9001)
* Web interface (HTTPS): [https://your-nas-ip:9002](https://your-nas-ip:9002)

## Device adoption

When running in bridge network mode (the default), the controller must know its own external IP so it can advertise the correct inform URL to devices. Set `SYSTEM_IP` to the NAS IP address in your `.env` file.

Without this, the controller advertises its internal Docker IP (e.g. `172.x.x.x`) to devices, which they cannot reach from the LAN.

Once the stack is running, devices that have already been adopted with a different inform URL must be re-pointed manually via SSH:

```bash
set-inform http://<SYSTEM_IP>:9001/inform
```

## Notes

* Make sure ports 10001/UDP, 3478/UDP, etc. are not in use by other services.
* For optimal compatibility with UniFi devices, leave the exposed ports as defined unless you know what you're doing.
* The volumes ensure persistence of MongoDB data and UniFi configuration between reboots or updates.

---

Take full control of your network with your own UniFi Controller! 🌐🛡️

