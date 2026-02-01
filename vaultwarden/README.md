# Vaultwarden Password Manager for Synology

This project provides a self-hosted Vaultwarden server, ready to deploy on Synology NAS via Container Manager using Docker Compose.

## Overview

- Lightweight, self-hosted password manager compatible with Bitwarden clients
- Full support for official Bitwarden apps (mobile, desktop, browser extensions)
- Web-based vault interface for password management
- Health check monitoring for reliable operation
- Uses bind-mounted volumes for persistent data storage

## Configuration

All environment variables are defined in the `.env.example` file. Copy it to `.env` and modify the values:

```env
# Project
COMPOSE_PROJECT_NAME=password-manager

# Docker image version
VAULTWARDEN_VERSION=latest

# Vaultwarden CONFIG
VAULTWARDEN_HTTP_PORT=9005
TZ=Europe/Madrid

# Paths
VAULTWARDEN_DATA_VOLUME_PATH=/volume1/docker/cybersecurity/password-manager/volumes/vaultwarden-data
```

> **Important**: Vaultwarden stores sensitive credential data. It is strongly recommended to:
> - Use HTTPS via Synology Reverse Proxy for all access
> - Keep regular backups of your data volume
> - Restrict network access to trusted clients only

## Directory Structure

```
/volume1/docker/cybersecurity/password-manager/
├── docker-compose.yml
├── .env
└── volumes/
    └── vaultwarden-data/    ← Vault database, attachments, and configuration
```

## How to Deploy (Synology Container Manager)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the path containing your `docker-compose.yml` and `.env.example` files.
5. Copy `.env.example` to `.env` and modify the values as needed.
6. Click **Create** to launch the project.

> Ensure that all volume paths exist before deployment to avoid bind errors.

## Access

* Web Vault: [http://your-nas-ip:9005](http://your-nas-ip:9005)

On first launch, you can create your account directly from the web interface. Once registered, you can use any official Bitwarden client to connect to your server.

### Connecting Bitwarden Clients

To use official Bitwarden apps with your Vaultwarden server:

1. Open the Bitwarden app (mobile, desktop, or browser extension)
2. Click the **Settings** gear icon before logging in
3. Enter your server URL (e.g., `https://vault.example.com` or `http://your-nas-ip:9005`)
4. Save and log in with your credentials

## Health Check

The service includes automatic health monitoring:

- **Method**: Health endpoint verification using `curl -f http://localhost:80/alive`
- **Interval**: 1 minute
- **Timeout**: 30 seconds
- **Retries**: 3 attempts
- **Start Period**: 2 minutes

## Security Recommendations

Since Vaultwarden manages sensitive credentials, consider implementing these security measures:

1. **Enable HTTPS**: Configure Synology Reverse Proxy (Control Panel > Application Portal > Reverse Proxy) to provide SSL/TLS encryption.

2. **Restrict Registration**: After creating your account(s), you can disable new user registration by adding to your environment:
   ```env
   SIGNUPS_ALLOWED=false
   ```

3. **Enable Admin Panel** (optional): For advanced configuration, you can enable the admin panel:
   ```env
   ADMIN_TOKEN=your-secure-admin-token
   ```
   Access it at `/admin` after setting the token.

4. **Regular Backups**: Back up the `vaultwarden-data` volume regularly using Synology Hyper Backup or similar tools.

## Notes

- Vaultwarden is fully compatible with all official Bitwarden clients and browser extensions.
- The data volume contains the SQLite database, attachments, and RSA keys - protect it carefully.
- You can modify the port by updating `VAULTWARDEN_HTTP_PORT` in the `.env` file.
- For additional configuration options, refer to the [Vaultwarden Wiki](https://github.com/dani-garcia/vaultwarden/wiki).
- Health checks provide automatic monitoring and recovery.

---

Secure your passwords with your own self-hosted Vaultwarden server!
