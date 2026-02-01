# Synology Photos Video Enhancer for Synology

This project provides a self-hosted video enhancer for Synology Photos, ready to deploy on Synology NAS via Container Manager using Docker Compose.

## Overview

- Automatically improves quality of Synology Photos intermediate videos
- Fixes the poor 15 fps / H.264 baseline transcodes created by Synology Photos
- Hardware acceleration support (Intel QSV/VAAPI, AMD VAAPI)
- Configurable output codec, bitrate, and resolution
- SQLite database for tracking transcoded videos
- Periodic execution at configurable intervals

## The Problem

Synology Photos generates intermediate videos with very poor quality:
- **H.264 baseline profile** (inefficient)
- **15 fps framerate** (causes stuttering on Chromecast and other devices)
- **Large file sizes** for the quality provided

This tool re-transcodes those intermediate videos to modern formats (H.264 High or HEVC) with proper framerates and better compression.

## Configuration

All environment variables are defined in the `.env.example` file. Copy it to `.env` and modify the values:

```env
# Project
COMPOSE_PROJECT_NAME=photo-video-enhancer

# Docker image version (stable, latest, or specific tag like v3.0.0)
DOCKER_VERSION=stable

# Database persistence
DATABASE_HOST_PATH=/volume1/docker/photo/video-enhancer/volumes/data

# Transcoding Resources
HW_TRANSCODING=True
EXECUTION_THREADS=2
STARTUP_DELAY=30
EXECUTION_INTERVAL=240

# Output Video Settings
VIDEO_CODEC=h264
VIDEO_BITRATE=2048
VIDEO_RESOLUTION=720p
VIDEO_PROFILE=high

# Output Audio Settings
AUDIO_CODEC=aac
AUDIO_BITRATE=128
AUDIO_CHANNELS=2
AUDIO_PROFILE=aac_lc

# Logging
LOGGER_NAME=video-enhancer
LOGGER_LEVEL=INFO
```

> **Important**: You must also edit `docker-compose.yml` to configure the photo directory mounts for your users.

## Directory Structure

```
/volume1/docker/photo/video-enhancer/
├── docker-compose.yml
├── .env
└── volumes/
    └── data/                ← SQLite database for tracking transcoded videos
        └── transcodings.db
```

## Photo Directory Configuration

Edit `docker-compose.yml` to mount your photo directories:

```yaml
volumes:
  # Common photos folder (if you have one)
  - /volume1/photo:/media/photos:rw

  # Individual user folders - add one line per user
  - /volume1/homes/john/Photos:/media/john:rw
  - /volume1/homes/jane/Photos:/media/jane:rw
```

Replace `john`, `jane`, etc. with your actual Synology usernames.

## How to Deploy (Synology Container Manager)

1. Open **Container Manager** in DSM.
2. Go to the **Projects** tab.
3. Click **Create**.
4. Select the path containing your `docker-compose.yml` and `.env.example` files.
5. Copy `.env.example` to `.env` and modify the values as needed.
6. **Edit `docker-compose.yml`** to configure your photo directory mounts.
7. Click **Create** to launch the project.

> Ensure that all volume paths exist before deployment to avoid bind errors.

## Prerequisites

- **Synology Photos** > 1.8
- **DSM** > 7.1
- **Intel CPUs**: Install [SynoCli Video Drivers](https://synocommunity.com/) package (> 1.3) for hardware acceleration
- **AMD CPUs**: Drivers included by default

## How It Works

1. **Startup Delay**: Waits `STARTUP_DELAY` minutes after container start
2. **Periodic Scanning**: Runs every `EXECUTION_INTERVAL` minutes
3. **Video Detection**: Scans all mounted photo directories for videos
4. **Metadata Reading**: Reads original video metadata from Synology's `SYNOINDEX_MEDIA_INFO` files
5. **Smart Transcoding**: Only transcodes videos that haven't been processed
6. **Output Storage**: Saves enhanced videos to `@eaDir/[video_name]/SYNOPHOTO_FILM_H.mp4`

## Environment Variables

### Transcoding Resources

| Variable | Default | Description |
|----------|---------|-------------|
| `HW_TRANSCODING` | `True` | Enable hardware acceleration |
| `EXECUTION_THREADS` | `2` | FFmpeg threads (max: half of CPU cores) |
| `STARTUP_DELAY` | `30` | Minutes to wait before first execution |
| `EXECUTION_INTERVAL` | `240` | Minutes between executions |

### Video Settings

| Variable | Default | Options |
|----------|---------|---------|
| `VIDEO_CODEC` | `h264` | `h264`, `hevc`, `mpeg4`, `vp8`, `vp9`, `av1` |
| `VIDEO_BITRATE` | `2048` | Bitrate in kbps |
| `VIDEO_RESOLUTION` | `720p` | `144p` to `2160p` |
| `VIDEO_PROFILE` | `high` | H.264: `baseline`, `main`, `high`; HEVC: `main`, `main10` |

### Audio Settings

| Variable | Default | Options |
|----------|---------|---------|
| `AUDIO_CODEC` | `aac` | `aac`, `mp3`, `ac3`, `opus`, `vorbis`, `flac` |
| `AUDIO_BITRATE` | `128` | Bitrate in kbps |
| `AUDIO_CHANNELS` | `2` | `1` (mono), `2` (stereo) |

## Monitoring

After each execution, the application logs a summary:

```
2025-01-04T12:01:35+0000 (video-enhancer) INFO | Processing results:
2025-01-04T12:01:35+0000 (video-enhancer) INFO |   - Total processed: 514
2025-01-04T12:01:35+0000 (video-enhancer) INFO |   - Already transcoded: 239
2025-01-04T12:01:35+0000 (video-enhancer) INFO |   - Transcoded: 275
2025-01-04T12:01:35+0000 (video-enhancer) INFO |   - Errors: 0
```

View logs via Container Manager or with `docker logs photo-video-enhancer_app`.

## Notes

- The tool only processes videos that haven't been enhanced yet (smart detection).
- Hardware acceleration significantly speeds up transcoding and reduces CPU load.
- For Intel CPUs, you must install SynoCli Video Drivers from SynoCommunity.
- The database tracks all transcoding operations to avoid reprocessing.
- For optional Grafana monitoring, refer to the [official documentation](https://github.com/cibrandocampo/synology-photos-video-enhancer).

---

Fix your Synology Photos video quality with automatic re-transcoding!
