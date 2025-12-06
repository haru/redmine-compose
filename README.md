# Redmine Compose

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A simple Docker Compose setup to quickly deploy Redmine with PostgreSQL.

## Features

- 🚀 One-command deployment
- 🐘 PostgreSQL database
- 🔧 Easy configuration via `.env` file
- 📁 Persistent data storage
- 🎨 Custom themes support
- 🔌 Plugin support

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Quick Start

1. **Clone this repository**

   ```bash
   git clone https://github.com/haru/redmine-compose.git
   cd redmine
   ```

2. **Configure environment variables**

   Copy the example `.env` file and edit as needed:

   ```bash
   cp .env.example .env
   ```

   Generate a secret key and update `.env`:

   ```bash
   openssl rand -hex 64
   ```

3. **Start Redmine**

   ```bash
   docker compose up -d
   ```

4. **Access Redmine**

   Open your browser and navigate to: http://localhost:3000

   Default login credentials:
   - **Username:** `admin`
   - **Password:** `admin`

   > ⚠️ Please change the default password immediately after first login.

## Configuration

### Environment Variables

Edit the `.env` file to customize your setup:

| Variable | Description | Default |
|----------|-------------|---------|
| `REDMINE_VERSION` | Redmine Docker image version | `latest` |
| `REDMINE_PORT` | Port to access Redmine | `3000` |
| `DB_USER` | PostgreSQL username | `redmine` |
| `DB_PASSWORD` | PostgreSQL password | `password` |
| `DB_NAME` | PostgreSQL database name | `redmine` |
| `REDMINE_SECRET_KEY_BASE` | Secret key for session encryption | (required) |

### Generate Secret Key

Generate a secure secret key using one of the following methods:

```bash
# Using OpenSSL (recommended)
openssl rand -hex 64

# Using Redmine's rake task
docker compose run --rm redmine bundle exec rake secret
```

## Directory Structure

```
.
├── docker-compose.yml    # Docker Compose configuration
├── .env                  # Environment variables
├── db_data/              # PostgreSQL data (persistent)
├── files/                # Redmine uploaded files (persistent)
├── plugins/              # Redmine plugins
└── themes/               # Redmine themes
```

## Adding Plugins

Place your plugins in the `plugins/` directory:

```bash
cd plugins
git clone <plugin-repository-url>
```

Then restart Redmine:

```bash
docker compose restart redmine
```

> Plugin migrations are automatically executed on container restart.

## Adding Themes

Place your themes in the `themes/` directory:

```bash
cd themes
git clone <theme-repository-url>
```

After adding a theme, go to **Administration → Settings → Display** in Redmine to select it.

> ⚠️ **For Redmine < 6.0:** The themes directory location is different. You need to change the mount point in `docker-compose.yml`:
>
> ```yaml
> # Change this line:
> - ./themes:/usr/src/redmine/themes
> # To:
> - ./themes:/usr/src/redmine/public/themes
> ```

## Common Commands

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f

# View Redmine logs only
docker compose logs -f redmine

# Restart Redmine
docker compose restart redmine

# Access Redmine container shell
docker compose exec redmine bash

# Run Redmine rake tasks
docker compose exec redmine bundle exec rake <task>
```

## Backup

All data is stored in this directory. Simply back up the entire project folder to preserve:

- `db_data/` - PostgreSQL database
- `files/` - Uploaded files
- `plugins/` - Installed plugins
- `themes/` - Installed themes
- `.env` - Configuration

```bash
# Example: Create a backup archive
tar -czvf redmine-backup-$(date +%Y%m%d).tar.gz .
```

## Upgrading Redmine

> ⚠️ Always back up your data before upgrading.

### If using `REDMINE_VERSION=latest`

```bash
docker compose pull
docker compose down
docker compose up -d
```

### If using a specific version (e.g., `REDMINE_VERSION=6.0`)

1. Edit `.env` and update the version:

   ```env
   REDMINE_VERSION=6.1
   ```

2. Pull the new image and restart:

   ```bash
   docker compose pull
   docker compose down
   docker compose up -d
   ```

## License

This project is licensed under the [MIT License](LICENSE).

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
