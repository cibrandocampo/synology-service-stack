# Jellyfin Media Server for Synology

This project provides a self-hosted Jellyfin media server, ready to deploy on Synology NAS via Container Manager using Docker Compose.

## Overview

- Self-hosted media server for movies, TV shows, music, and more
- Hardware acceleration support (Intel QuickSync / VAAPI via GPU passthrough)
- Web-based interface for media management and playback
- Health check monitoring for reliable operation
- Uses bind-mounted volumes for persistent configuration and media storage

## Configuration

All environment variables are defined in the `.env.example` file. Copy it to `.env` and modify the values:

```env
# Project
COMPOSE_PROJECT_NAME=media-jellyfin

# Docker image version
JELLYFIN_VERSION=latest

# Port mapping
JELLYFIN_HTTP_PORT=8096
JELLYFIN_HTTPS_PORT=8920

# Timezone
TZ=Europe/Madrid

# Published server URL (for external access)
# Examples:
#   - Direct access: http://192.168.1.100:8096
#   - With domain: https://jellyfin.example.com:8096
#   - With reverse proxy (no port needed): https://jellyfin.example.com
JELLYFIN_PUBLISHED_URL=http://your-nas-ip:8096

# Volume paths (adjust to your folder structure)
JELLYFIN_CONFIG=/volume1/docker/media/jellyfin/volumes/config
JELLYFIN_CACHE=/volume1/docker/media/jellyfin/volumes/cache
MEDIA_ROOT=/volume1/media
```

> ⚠️ Important: The `JELLYFIN_PUBLISHED_URL` is used by Jellyfin to generate proper links for external access. You can use:
> - **IP address with port**: `http://192.168.1.100:8096` (for direct access)
> - **Domain name with port**: `https://jellyfin.example.com:8096` (if accessing via domain)
> - **Domain name without port**: `https://jellyfin.example.com` (if using Synology Reverse Proxy)
> 
> If you configure Synology's Reverse Proxy, you typically won't need to include the port number in the URL.

## Directory Structure

```
/volume1/docker/media/jellyfin/
├── docker-compose.yml
├── .env
└── volumes/
    ├── config/          ← Jellyfin configuration and database
    └── cache/           ← Transcoding cache and temporary files

/volume1/media/          ← Your media library (movies, TV shows, music, etc.)
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

* Web UI (HTTP): [http://your-nas-ip:8096](http://your-nas-ip:8096)
* Web UI (HTTPS): [https://your-nas-ip:8920](https://your-nas-ip:8920)

On first launch, you'll be guided through the initial setup wizard to configure your media library and admin account.

## Hardware Acceleration

The container is configured with GPU passthrough (`/dev/dri`) to enable hardware-accelerated transcoding using Intel QuickSync or VAAPI. This requires:

- A Synology NAS with Intel CPU that supports QuickSync
- Proper permissions for `/dev/dri` device access
- Hardware acceleration enabled in Jellyfin settings after initial setup

## Health Check

The service includes automatic health monitoring:

- **Method**: Health endpoint verification using `curl -f http://localhost:8096/health`
- **Interval**: 1 minute
- **Timeout**: 30 seconds
- **Retries**: 3 attempts

## Published Server URL Configuration

The `JELLYFIN_PUBLISHED_URL` environment variable tells Jellyfin how to generate URLs for external access. This is important for:

- **Remote access**: Clients need to know how to reach your server
- **Mobile apps**: Proper URL generation for mobile device connections
- **Sharing**: Correct links when sharing media with others

### Configuration Options

1. **Direct IP access** (local network):
   ```
   JELLYFIN_PUBLISHED_URL=http://192.168.1.100:8096
   ```

2. **Domain with port** (external access):
   ```
   JELLYFIN_PUBLISHED_URL=https://jellyfin.example.com:8096
   ```

3. **Domain via Reverse Proxy** (recommended for Synology):
   ```
   JELLYFIN_PUBLISHED_URL=https://jellyfin.example.com
   ```
   When using Synology's Reverse Proxy (Control Panel → Application Portal → Reverse Proxy), you typically configure it to forward requests to `localhost:8096` without exposing the port in the public URL.

## Notes

- The `MEDIA_ROOT` volume should point to your actual media library location on your Synology.
- Hardware acceleration requires compatible hardware and proper device permissions.
- You can modify the ports by updating `JELLYFIN_HTTP_PORT` and `JELLYFIN_HTTPS_PORT` in the `.env` file.
- The `JELLYFIN_PUBLISHED_URL` can use either IP address or domain name, and should omit the port if using Reverse Proxy.
- The config and cache volumes ensure persistence of Jellyfin settings and transcoding data.
- Health checks provide automatic monitoring and recovery.

---

Stream your media collection from anywhere with your own Jellyfin server! 🎬🎵

