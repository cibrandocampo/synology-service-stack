# synology-service-stack

**Ready-to-deploy dockerized services: the must-have stack for Synology**

This repository contains a collection of essential services, each dockerized and preconfigured to run on a Synology NAS using Docker Compose through the Synology **Container Manager**.

All services are modular and include:

- Predefined `.env` variables
- Persistent volume mapping
- Deployment instructions for Synology users

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

---

## How to Use (on Synology)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the folder containing the `docker-compose.yml` and `.env` files for the service you want.
5. Click **Create** to launch the project.

> ℹ️ Each subproject contains its own `README.md` file with specific setup details, volumes, and configuration tips.

---

## Repository Structure

```

synology-service-stack/
├── nginx/
├── pihole/
├── speedtest/
├── unifi/
├── wordpress/
├── dyndns/
└── README.md

```

---

## Notes

- All volumes are bind-mounted to preserve data across reboots and updates.
- Before deploying a service, make sure the volume paths defined in the `.env` file exist on your Synology.
- Services can be deployed independently and side by side.

---

Made for Synology users who want full control of their self-hosted environment. 🧰📡  
**Happy hosting!**
```