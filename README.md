# SALDI

SALDI is a free Danish accounting system designed to manage various financial operations for businesses.
[Website: saldi.dk](https://saldi.dk)
## Features

The repository includes various directories and files essential for the system's functionality:

- [admin/](src/admin/): Administrative tools and scripts.
- [api/](src/api/): API-related functionalities.
- [booking/](src/booking/): Booking management components.
- [debitor/](src/debitor/): Debtor management modules.
- [finans/](src/finans/): Financial management tools.
- [lager/](src/lager/): Inventory management features.
- [produktion/](src/produktion/): Production management modules.
- [systemdata/](src/systemdata/): System data configurations.

## About SALDI

The system is primarily documented in Danish, and currently, most of it have been translated to English and Norse. Danish-speaking users can refer to the [`LAESMIG.txt`](LAESMIG.txt) file included in the repository for more information.

## Getting Started

To use SALDI, clone the repository:


## Installation

Choose one of the following methods:

### Docker (Recommended)

SALDI is available as a Docker image: `danosoft/saldi:latest`

#### Quick start

1. Create a folder for SALDI and copy the files you need:

```bash
mkdir saldi && cd saldi
```

2. Copy a compose file from [docker/docker-compose/](docker/docker-compose/) to your folder and rename it.
   This folder contains all compose examples and the [.env.example](docker/docker-compose/.env.example) file:

| Source file | Use case |
|-------------|----------|
| [docker-compose.prod-psql.yml](docker/docker-compose/docker-compose.prod-psql.yml) | Production with bundled PostgreSQL 15 database |
| [docker-compose.prod-nosql.yml](docker/docker-compose/docker-compose.prod-nosql.yml) | Production with your own external database |
| [docker-compose.dev-psql.yml](docker/docker-compose/docker-compose.dev-psql.yml) | Development with bundled PostgreSQL (mounts source code) |
| [docker-compose.dev-nosql.yml](docker/docker-compose/docker-compose.dev-nosql.yml) | Development with external database (mounts source code) |

```bash
cp docker-compose.prod-psql.yml docker-compose.yml
```

3. Copy and configure the environment file:

```bash
cp .env.example .env
```

Edit `.env` with your settings:

```env
SALDI_DOCKER=1          # Required - enables Docker mode
SALDI_PORT=8080         # Port mapping (default 8080)
DB_TYPE=postgresql      # postgresql or mysqli
DB_HOST=db              # Use 'db' for bundled database, or your database hostname
DB_NAME=saldi           # Database name
DB_USER=saldi           # Database user
DB_PASSWORD=changeme    # CHANGE THIS!
```

4. Start SALDI:

```bash
docker compose up -d
```

5. Open http://localhost:8080 (or your configured SALDI_PORT) to complete the setup wizard.

#### Images used

- `danosoft/saldi:latest` - SALDI application (PHP 8.2 + Apache)
- `postgres:15` - PostgreSQL database (only with psql configurations)

#### Persistent data

These directories are created automatically for your data:
- `saldibilag/` - Uploaded documents
- `backup/` - Database backups
- `saldi_logolib/` - Company logos

#### More information

See [docker/README.md](docker/README.md) for detailed Docker documentation, including environment variables, troubleshooting, and how Docker mode works.

### Manual Installation

See [INSTALLATION.txt](INSTALLATION.txt) for traditional server setup.

```bash
git clone https://github.com/DANOSOFT/saldi.git
```
