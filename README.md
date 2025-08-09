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

3. **Create required directories with proper permissions:**
   ```bash
   sudo mkdir -p /var/openproject/assets
   sudo chown 1000:1000 -R /var/openproject/assets
   ```

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
```bash
docker compose down --volumes
sudo rm -rf /var/openproject/assets /var/lib/postgresql/data
```

**Alternative method - Remove specific volumes:**
```bash
docker compose down
docker volume rm openproject-docker-compose_pgdata openproject-docker-compose_opdata
sudo rm -rf /var/openproject/assets /var/lib/postgresql/data
```

**Note:** After a fresh installation, you'll get the default `admin/admin` credentials again.

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
- **Most Common:** Wait 30-60 seconds for full startup - OpenProject takes time to initialize all services
- Check logs: `docker compose logs web`
- Verify all containers are running: `docker compose ps`

### Port Already in Use ("Bind for 127.0.0.1:8080 failed: port is already allocated")
This happens when Docker's port allocation gets stuck from previous runs:

```bash
# Step 1: Stop all containers and clean up
docker compose down
docker stop $(docker ps -aq) 2>/dev/null || true
docker rm $(docker ps -aq) 2>/dev/null || true

# Step 2: Clean up Docker networks
docker network prune -f

# Step 3: Restart Docker service
sudo service docker stop
sudo service docker start

# Step 4: Verify port is free
sudo lsof -i :8080

# Step 5: Try starting again
docker compose up -d --build --pull always
```

**Alternative:** If using Docker Desktop, restart it completely from the system tray.

### Permission Issues
```bash
sudo chown 1000:1000 -R /var/openproject/assets
```

### Change Port
Edit `.env` file and modify both:
```
OPENPROJECT_HOST__NAME=127.0.0.1:YOUR_PORT
PORT=127.0.0.1:YOUR_PORT
```

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
**Repository**: https://github.com/mblackonline/openproject-docker-compose
