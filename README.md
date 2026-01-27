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

## Installation

### Docker (Recommended)

SALDI is available as a Docker image: `danosoft/saldi:latest`

#### Production Setup

No need to clone the repository. Just download two files:

1. Choose your setup:

| Option | Use when |
|--------|----------|
| `docker-compose.prod-psql.yml` | You want SALDI to run its own PostgreSQL database |
| `docker-compose.prod-nosql.yml` | You have an existing database server (PostgreSQL or MySQL) |

2. Create a folder and download the compose file and environment template:

**With bundled PostgreSQL (recommended for new installations):**
```bash
mkdir saldi && cd saldi
curl -O https://raw.githubusercontent.com/DANOSOFT/saldi/master/docker/docker-compose/docker-compose.prod-psql.yml
curl -O https://raw.githubusercontent.com/DANOSOFT/saldi/master/docker/docker-compose/.env.example
mv docker-compose.prod-psql.yml docker-compose.yml
mv .env.example .env
```

**With external database:**
```bash
mkdir saldi && cd saldi
curl -O https://raw.githubusercontent.com/DANOSOFT/saldi/master/docker/docker-compose/docker-compose.prod-nosql.yml
curl -O https://raw.githubusercontent.com/DANOSOFT/saldi/master/docker/docker-compose/.env.example
mv docker-compose.prod-nosql.yml docker-compose.yml
mv .env.example .env
```

3. Edit `.env` with your settings:

```env
GENERATE_CONNECT_PHP=1  # Auto-generate connect.php (set 0 for manual)
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

#### Available compose files

| File | Use case |
|------|----------|
| [docker-compose.prod-psql.yml](docker/docker-compose/docker-compose.prod-psql.yml) | Production with bundled PostgreSQL 15 |
| [docker-compose.prod-nosql.yml](docker/docker-compose/docker-compose.prod-nosql.yml) | Production with external database |

#### Development Setup

For development, clone the repository to get source code access:

```bash
git clone https://github.com/DANOSOFT/saldi.git
cd saldi/docker/docker-compose
cp .env.example .env
# Edit .env with your settings
docker compose -f docker-compose.dev-psql.yml up -d
```

Development compose files mount the source code for live editing:
- [docker-compose.dev-psql.yml](docker/docker-compose/docker-compose.dev-psql.yml)
- [docker-compose.dev-nosql.yml](docker/docker-compose/docker-compose.dev-nosql.yml)

#### More information

See [docker/README.md](docker/README.md) for detailed documentation on environment variables, troubleshooting, and how Docker mode works.

### Manual Installation

For traditional server setup without Docker, clone the repository and follow [INSTALLATION.txt](INSTALLATION.txt):

```bash
git clone https://github.com/DANOSOFT/saldi.git
```
