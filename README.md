# OpenProject Docker Template

> **Based on [OpenProject Docker Compose](https://github.com/opf/openproject-docker-compose)**  
> This is a simplified template for quickly setting up OpenProject for development teams.

A clean Docker Compose template for running OpenProject locally for development and testing.

## About OpenProject

[OpenProject](https://www.openproject.org/) is the leading open source project management software, licensed under GPL v3. This template is based on the official OpenProject Docker Compose setup.

## Quick Start

### Prerequisites
- Docker and Docker Compose installed
- Git

**Tested Environment:**
- WSL2 Ubuntu 22.04 with Docker Desktop
- This template has been specifically tested and verified on this setup

### Setup Instructions

1. **Clone this repository:**
   ```bash
   git clone https://github.com/mblackonline/openproject-docker-compose.git
   cd openproject-docker-compose
   ```

2. **Create your environment file:**
   ```bash
   cp .env.example .env
   ```

3. **Create required directories:**
   ```bash
   mkdir -p ./pgdata ./opdata
   sudo chown 1000:1000 ./opdata
   ```
   
   **Note:** This template uses project-specific data directories for safety. Data will be stored in your project folder, not system-wide locations.

4. **Start OpenProject:**
   ```bash
   docker compose up -d --build --pull always
   ```

5. **Start OpenProject and wait for full startup:**
   ```bash
   # Wait 30-60 seconds for all services to fully start
   # You will get 502 errors if you try to access too early
   ```
   
6. **Access OpenProject:**
   - URL: http://127.0.0.1:8080
   - Username: `admin`
   - Password: `admin`
   - **Note:** Wait at least 30-60 seconds after startup before accessing. You'll get 502 Bad Gateway errors if services aren't fully ready.

### Management Commands

**Stop OpenProject:**
```bash
docker compose stop
```

**Start OpenProject:**
```bash
docker compose start
```

**View logs:**
```bash
docker compose logs -f
```

**Complete teardown (removes containers but keeps data):**
```bash
docker compose down
```

**Fresh installation (removes ALL data):**

**If using project-specific directories (recommended):**
```bash
docker compose down
sudo rm -rf ./pgdata ./opdata

# If you still get port conflicts:
sudo service docker restart
```

**If using default system directories (⚠️ DANGEROUS):**
```bash
docker compose down
# WARNING: This may delete data from other PostgreSQL projects!
sudo rm -rf /var/lib/postgresql/data /var/openproject/assets
```

**Note:** After a fresh installation, you'll get the default `admin/admin` credentials again.

## Configuration

### Changing Port
Edit `.env` file and modify both:
```
OPENPROJECT_HOST__NAME=127.0.0.1:YOUR_PORT
PORT=127.0.0.1:YOUR_PORT
```

## Troubleshooting

### 502 Bad Gateway
Wait 30-60 seconds for startup. Check logs: `docker compose logs web`

### Port Already in Use
```bash
docker compose down
sudo service docker restart
docker compose up -d
```

### Permission Issues
```bash
sudo chown 1000:1000 ./opdata
```

## Security Notes

- Default credentials are `admin/admin` - **change after first login for any shared environments**
- This setup is intended for local development and testing only

## Attribution

This template is based on the official [OpenProject Docker Compose](https://github.com/opf/openproject-docker-compose) repository.

- **OpenProject**: https://www.openproject.org/
- **Original Repository**: https://github.com/opf/openproject-docker-compose
- **License**: GPL v3
- **Documentation**: https://www.openproject.org/docs/installation-and-operations/installation/docker-compose/

## Support

- [OpenProject Documentation](https://www.openproject.org/docs/)
- [Docker Installation Guide](https://www.openproject.org/docs/installation-and-operations/installation/docker-compose/)
- [OpenProject Community](https://community.openproject.org/)

---

**Template created by**: Matt Black  
**Repository**: https://github.com/mblackonline/openproject-docker-compose
