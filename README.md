# Self-Hosted Media & Photo Infrastructure

Jellyfin for media serving and Immich for photo management, both containerized and running continuously. Immich includes machine learning-based photo recognition through a dedicated ML container.

## Problem

Cloud photo and media storage means recurring cost, third-party access to personal libraries, and no real control over retention or compression. A self-hosted stack removes the dependency entirely, at the cost of owning the uptime and backup story yourself.

## Architecture

- Jellyfin handles media serving — library scanning, transcoding, and streaming to clients
- Immich handles photo management, with a dedicated ML container running facial recognition and object detection against the photo library
- PostgreSQL backs Immich's metadata and search; Valkey (Redis-compatible) handles caching and job queueing
- Library naming follows a consistent convention (`Show Name S01E01`, `Season ##`) so Jellyfin's metadata scraper resolves shows correctly without manual fixes
- Offsite backup runs through rclone with client-side encryption to Backblaze B2, so the original photo library and an encrypted copy never share the same point of failure

## Stack

Docker · Jellyfin · Immich · Valkey · PostgreSQL

## Outcome

Fully self-hosted media and photo library with no third-party cloud dependency, and an encrypted offsite backup path independent of the primary NAS.

## Notes

This repo documents the architecture pattern, not the live configuration. Volume paths, the ML model settings, and backup credentials in `docker-compose.yml` and `.env.example` are placeholders — copy `.env.example` to `.env` and fill in real values before deploying.
