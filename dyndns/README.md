# OVH DynDNS Client for Synology

This project provides a self-hosted DynDNS client for OVH domains, ready to deploy on Synology NAS via Container Manager using Docker Compose.

## Overview

- Automatically updates OVH DNS records when your public IP changes
- Web interface for managing hosts, viewing status and history
- REST API with JWT authentication
- SQLite database for persistent storage
- Health check monitoring for reliable operation
- Uses bind-mounted volumes for persistent data storage

## Configuration

All environment variables are defined in the `.env.example` file. Copy it to `.env` and modify the values:

```env
# Project
COMPOSE_PROJECT_NAME=ovh-dyndns

# Docker image version
DOCKER_OVH_VERSION=stable

# API
API_PORT=8000

# Data persistence (SQLite database)
DATA_PATH=/volume1/docker/network/dyndns/volumes/data

# Security (change in production!)
JWT_SECRET=your-secure-random-secret-key-min-32-chars

# Logging
LOGGER_NAME=ovh-dyndns
LOGGER_LEVEL=INFO
```

> **Important**: Change `JWT_SECRET` and `ADMIN_PASSWORD` before deploying to production. The JWT secret should be at least 32 characters long.

## Directory Structure

```
/volume1/docker/network/dyndns/
├── docker-compose.yml
├── .env
└── volumes/
    └── data/                ← SQLite database and application data
        └── dyndns.db
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

* Web UI: [http://your-nas-ip:8000](http://your-nas-ip:8000)

Default credentials: `admin` / `admin` (password change required on first login)

## Web Interface

The web interface provides:

- **Status** - Current public IP and host update status
- **Hosts** - Add, edit, and delete DNS hosts to manage
- **History** - View all IP changes and DNS updates
- **Settings** - Configure update interval and log level

## Adding a Host

1. Log in to the web interface
2. Go to the **Hosts** section
3. Click **Add Host**
4. Enter your OVH DynHost credentials:
   - **Hostname**: Your full domain (e.g., `home.example.com`)
   - **Username**: DynHost username from OVH Control Panel
   - **Password**: DynHost password from OVH Control Panel

> To create DynHost credentials, go to OVH Control Panel > Domain > DynHost tab.

## Health Check

The service includes automatic health monitoring:

- **Method**: Health endpoint verification using `curl -f http://localhost:8000/api/health`
- **Interval**: 1 minute
- **Timeout**: 30 seconds
- **Retries**: 3 attempts
- **Start Period**: 30 seconds

## How It Works

1. Retrieves current public IP using [ipify](https://www.ipify.org/)
2. Compares with stored IP in SQLite database
3. If changed, updates all configured OVH DNS records via DynHost API
4. Failed updates are tracked and retried automatically on the next cycle

## Security Recommendations

1. **Change default credentials**: The default `admin/admin` should be changed immediately on first login.

2. **Set a strong JWT secret**: Use a random string of at least 32 characters.

3. **Enable HTTPS**: Configure Synology Reverse Proxy (Control Panel > Application Portal > Reverse Proxy) to provide SSL/TLS encryption.

4. **Restrict access**: Limit network access to trusted clients if possible.

## Runtime Settings

These settings can be changed through the web interface without restarting the container:

| Setting | Default | Range | Description |
|---------|---------|-------|-------------|
| Update Interval | 300 | 60-86400 | How often to check for IP changes (seconds) |
| Log Level | INFO | DEBUG, INFO, WARNING, ERROR, CRITICAL | Logging verbosity |

## Notes

- The data volume contains the SQLite database with your hosts and history.
- You can modify the port by updating `API_PORT` in the `.env` file.
- For more information, refer to the [OVH DynDNS Client documentation](https://github.com/cibrandocampo/ovh-dyndns-client).
- Health checks provide automatic monitoring and recovery.

---

Keep your OVH domains always pointing to your dynamic IP!
