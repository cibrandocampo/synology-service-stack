# OVH Dynamic DNS Updater for Synology

This project provides a ready-to-deploy containerized client for [OVH Dynamic DNS](https://github.com/cibrandocampo/ovh-dyndns-client), designed to run on Synology NAS via Container Manager.

It automatically updates your OVH DNS records when your public IP address changes — ideal for home networks with dynamic IPs.

## Overview

- Periodically checks your public IP and updates OVH DNS records.
- Minimal setup with a single `hosts.json` configuration file.
- Runs continuously in a lightweight container.
- Perfect for Synology NAS with OVH-managed domains.

## Configuration

All variables are defined in the `.env` file:

```env
# Project
COMPOSE_PROJECT_NAME=ovh-dyndns

# Docker image version
DOCKER_OVH_VERSION=2.0.0

# General config
HOSTS_CONFIG_FILE_PATH=/volume1/docker/network/dyndns/volumes/hosts.json
UPDATE_INTERVAL=300  # Interval in seconds

# Logger settings
LOGGER_NAME=ovh-dyndns
LOGGER_LEVE=INFO
````

You also need a `hosts.json` file defining the OVH domains and credentials.

**Configuration guide**: Refer to the official documentation: [https://github.com/cibrandocampo/ovh-dyndns-client](https://github.com/cibrandocampo/ovh-dyndns-client)

## Directory Structure

```
/volume1/docker/network/dyndns/
├── docker-compose.yml
├── .env
└── volumes/
    └── hosts.json      ← Your domain and authentication config
```

## How to Deploy (Synology Container Manager)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the folder containing your `docker-compose.yml`, `.env`, and `hosts.json`.
5. Click **Create** to launch the project.

> ℹ️ Ensure `hosts.json` is correctly configured and accessible at the path specified in `.env`.

## Notes

* The client runs in the background and updates OVH DNS records every X seconds (`UPDATE_INTERVAL`).
* Logs can be monitored from Container Manager or redirected as needed.
* Make sure your OVH account is properly configured with API access and valid credentials.

---

Keep your domain always reachable — even with a dynamic IP. 🌍🛰️
Powered by [cibrandocampo/ovh-dyndns-client](https://github.com/cibrandocampo/ovh-dyndns-client)
