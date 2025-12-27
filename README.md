# synology-service-stack

**Ready-to-deploy dockerized services: the must-have stack for Synology**

This repository contains a collection of essential services, each dockerized and preconfigured to run on a Synology NAS using Docker Compose through the Synology **Container Manager**.

All services are modular and include:

- Predefined `.env.example` files with secure defaults
- Persistent volume mapping with bind mounts
- Health checks for reliable monitoring
- Deployment instructions for Synology users
- Standardized Docker Compose configuration

---

## Included Services

| Service               | Description                                                    | Folder              |
|-----------------------|----------------------------------------------------------------|---------------------|
| **NGINX**             | Lightweight web server for serving static websites.            | [`nginx`](./nginx) |
| **OpenSpeedTest**     | Self-hosted network speed test interface.                      | [`speedtest`](./speedtest) |
| **WordPress + MariaDB** | Complete WordPress stack with database support.              | [`wordpress`](./wordpress) |
| **UniFi Controller**  | Network management interface for UniFi devices.                | [`unifi`](./unifi) |
| **Pi-hole**           | Network-wide DNS filtering and ad blocking.                    | [`pihole`](./pihole) |
| **OVH DynDNS Client** | Public IP updater for OVH-managed DNS domains.                 | [`dyndns`](./dyndns) |
| **Vaultwarden**         | Lightweight alternative to Bitwarden for password management. | [`vaultwarden`](./vaultwarden) |
| **Jellyfin**            | Self-hosted media server for movies, TV shows, and music.     | [`jellyfin`](./jellyfin) |

---

## Port Configuration

| Service | Default Port | Description |
|---------|---------------|-------------|
| **NGINX** | 8080 | Web server |
| **Pi-hole** | 9003 | DNS management interface |
| **WordPress** | 9200 | WordPress application |
| **UniFi Controller** | 9001 (HTTP), 9002 (HTTPS) | Network management |
| **Vaultwarden** | 9010 | Password manager |
| **OpenSpeedTest** | 9004 (HTTP), 9005 (HTTPS) | Network speed testing |
| **OVH DynDNS** | N/A | Background service |
| **Jellyfin** | 8096 (HTTP), 8920 (HTTPS) | Media server |

## How to Use (on Synology)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the folder containing the `docker-compose.yml` and `.env.example` files for the service you want.
5. Copy `.env.example` to `.env` and modify the values as needed.
6. Click **Create** to launch the project.

> Each subproject contains its own `README.md` file with specific setup details, volumes, and configuration tips.

---

## Repository Structure

```
synology-service-stack/
├── dyndns/
├── nginx/
├── pihole/
├── speedtest/
├── unifi/
├── vaultwarden/
├── wordpress/
├── jellyfin/
└── README.md
```

---

## Health Checks

All services include health checks for reliable monitoring:

- **NGINX**: HTTP endpoint verification
- **Pi-hole**: DNS service status check
- **WordPress**: Database connectivity verification
- **UniFi**: HTTP service verification
- **Vaultwarden**: Dedicated `/alive` endpoint
- **OpenSpeedTest**: HTTP service verification
- **DynDNS**: Configuration file verification
- **Jellyfin**: Health endpoint verification

## Security Features

- Secure default passwords in `.env.example` files
- Isolated network configurations
- Proper signal handling with `init: true`
- Health check monitoring for automatic recovery

## Notes

- All volumes are bind-mounted to preserve data across reboots and updates.
- Before deploying a service, make sure the volume paths defined in the `.env` file exist on your Synology.
- Services can be deployed independently and side by side.
- Health checks provide automatic monitoring and recovery.

---

Made for Synology users who want full control of their self-hosted environment.  