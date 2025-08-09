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
   git clone https://github.com/mblackonline/openproject.git
   cd openproject
   ```

2. **Create your environment file:**
   ```bash
   cp .env.template .env
   ```

3. **Create required directories with proper permissions:**
   ```bash
   sudo mkdir -p /var/openproject/assets
   sudo chown 1000:1000 -R /var/openproject/assets
   ```

4. **Start OpenProject:**
   ```bash
   docker compose up -d --build --pull always
   ```

5. **Wait for startup (30-60 seconds) then access:**
   - URL: http://127.0.0.1:8080
   - Username: `admin`
   - Password: `admin`

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
```bash
docker compose down
sudo rm -rf /var/openproject/assets /var/lib/postgresql/data
```

## Configuration

### Changing Port
Edit `.env` file and modify:
```
OPENPROJECT_HOST__NAME=127.0.0.1:YOUR_PORT
PORT=127.0.0.1:YOUR_PORT
```

### Production Use
For production deployment:
1. Change database password in `.env`
2. Set `OPENPROJECT_HTTPS=true`
3. Configure proper hostname
4. Set up proper SSL termination

## Data Persistence

Data is automatically persisted in:
- **Database:** `/var/lib/postgresql/data`
- **Files/Assets:** `/var/openproject/assets`

Data survives container restarts and system reboots.

## Troubleshooting

### 502 Bad Gateway
- Wait 30-60 seconds for full startup
- Check logs: `docker compose logs web`

### Permission Issues
```bash
sudo chown 1000:1000 -R /var/openproject/assets
```

### Port Already in Use
Change the port in `.env` file and restart.

## Security Notes

- Default credentials are `admin/admin` - **change immediately after first login**
- Database password should be changed for production use
- This setup is intended for development/testing, not production

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
**Repository**: https://github.com/mblackonline/openproject
