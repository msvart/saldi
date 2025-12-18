# SALDI Docker

This folder contains the Docker configuration for running SALDI in containers.

## Structure

```
docker/
├── Dockerfile              # Main image definition
├── entrypoint.sh           # Container startup script
├── php.ini                 # PHP configuration
├── vhost.conf              # Apache virtual host config
└── docker-compose/         # Compose files and environment
    ├── .env.example        # Environment template
    ├── docker-compose.prod-psql.yml    # Production with PostgreSQL
    ├── docker-compose.prod-nosql.yml   # Production with external DB
    ├── docker-compose.dev-psql.yml     # Development with PostgreSQL
    └── docker-compose.dev-nosql.yml    # Development with external DB
```

## Environment Variables

### Required

| Variable | Description |
|----------|-------------|
| `SALDI_DOCKER` | Must be set to `1` to enable Docker mode |
| `DB_TYPE` | Database type: `postgresql` or `mysqli` |
| `DB_HOST` | Database hostname (use `db` for bundled PostgreSQL) |
| `DB_NAME` | Database name |
| `DB_USER` | Database username |
| `DB_PASSWORD` | Database password |

### Optional

| Variable | Default | Description |
|----------|---------|-------------|
| `SALDI_PORT` | `8080` | Host port mapping |
| `DB_ENCODE` | `UTF8` | Database encoding |

## How Docker Mode Works

When `SALDI_DOCKER=1` is set, SALDI behaves differently:

### 1. Auto-generated connect.php

The `entrypoint.sh` script automatically generates `/var/www/html/includes/connect.php` from environment variables. This means:
- No manual database configuration needed
- Credentials stay in environment variables (not in code)
- connect.php is regenerated on each container start if missing

### 2. Schema Detection

Instead of checking for `connect.php` to detect installation status, Docker mode checks if the database schema exists (tables `regnskab` and `settings`). This prevents:
- Redirect loops on container restart
- Accidental reinstallation

### 3. Installer Redirect

If the schema doesn't exist, SALDI redirects to the web installer. The installer pre-fills database fields from environment variables.

## Image Details

**Base:** `php:8.2-apache`

**Included software:**
- PostgreSQL client
- ImageMagick, Ghostscript
- WeasyPrint (HTML to PDF)
- Barcode tools

**PHP extensions:**
- pgsql, pdo_pgsql
- gd (with FreeType, JPEG)
- mbstring, zip, bcmath

**Locale:** Danish (da_DK.UTF-8)

## Development vs Production

### Development (`dev-*`)
- Source code mounted from host for live editing
- Builds image locally from Dockerfile
- Named volumes for temp directories (permission handling)

### Production (`prod-*`)
- Uses pre-built `danosoft/saldi:latest` image
- Only persistent data directories mounted
- No source code access needed

## Volumes

| Path | Purpose |
|------|---------|
| `saldibilag/` | Uploaded documents and attachments |
| `backup/` | Database backup files |
| `saldi_logolib/` | Company logos |
| `pg_data` | PostgreSQL data (prod-psql only) |

## Troubleshooting

### Container won't start
Check that all required environment variables are set in `.env`.

### Permission issues
The entrypoint script adjusts www-data UID/GID to match the mounted volume owner. If issues persist, check volume ownership.

### Database connection fails
- Verify `DB_HOST` matches the service name (`db` for bundled PostgreSQL)
- Check that the database container is running
- Verify credentials in `.env`

### Redirect loop on install page
This usually means the schema check is failing. Check database connectivity and that the database exists.