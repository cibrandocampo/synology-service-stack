# Pi-hole DNS Server for Synology

This project provides a self-hosted Pi-hole DNS and ad-blocking server, ready to deploy on Synology NAS via Container Manager using Docker Compose.

## Overview

- Network-wide ad blocking and DNS filtering.
- Web UI for monitoring and configuration.
- Uses bind-mounted volumes for persistent configuration.
- Runs in `host` network mode to properly handle DNS resolution.

## Configuration

All environment variables are defined in the `.env` file:

```env
# Project
COMPOSE_PROJECT_NAME=network-dns

# Docker image version
PIHOLE_VERSION=2025.06.2

# Pi-hole configuration
FTLCONF_LOCAL_IPV4=10.70.2.20
WEB_PORT=9003
WEBPASSWORD=change-me
TZ=Europe/Madrid

# Volume paths
PIHOLE_DATA_VOLUME_PATH=/volume1/docker/network/dns/volumes/pihole-data
PIHOLE_DNSMASQ_VOLUME_PATH=/volume1/docker/network/dns/volumes/pihole-dnsmasq
````

> ⚠️ Important: You must provide the IP address of your Synology in `FTLCONF_LOCAL_IPV4` for DNS binding to work properly.

## Directory Structure

```
/volume1/docker/network/dns/
├── docker-compose.yml
├── .env
└── volumes/
    ├── pihole-data/       ← Pi-hole configuration and database
    └── pihole-dnsmasq/    ← Custom DNS rules and settings
```

## How to Deploy (Synology Container Manager)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the path containing your `docker-compose.yml` and `.env` files.
5. Click **Create** to launch the project.

> ℹ️ Ensure that the volume paths exist before deployment to avoid bind errors.

## Access

* Web UI: [http://your-nas-ip:9003/admin](http://your-nas-ip:9003/admin)
* Default login password: `change-me` (set via `WEBPASSWORD`)

## Notes

* The container runs in `host` mode to function properly as a DNS server.
* Make sure port `53` (DNS) is not used by other services on your NAS.
* It's recommended to assign a static IP to your NAS and use it as the primary DNS on your devices or router.
* Change the default `WEBPASSWORD` before going live.

---

Block ads and gain full control over your DNS traffic with Pi-hole on Synology! 🛡️🧠
