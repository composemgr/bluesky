## 👋 Welcome to bluesky 🚀

<<<<<<< HEAD
Self Hosted bluesky server

## Security Configuration

This PDS is configured with security best practices to prevent spam and abuse:

- **Invite-only registration** - Users need invite codes to create accounts
- **Email verification required** - All new accounts must verify their email
- **SSRF protection enabled** - Prevents internal network attacks
- **Strong credentials required** - No default passwords or secrets
- **Rate limiting** - Bluesky Relay enforces 10 accounts max, 1,500 events/hour per PDS

## Required Setup

Before starting the service, you MUST set these environment variables:

```bash
# Generate admin password
export APP_ADMIN_PASS=$(openssl rand -base64 32)

# Generate JWT secret
export APP_JWT_TOKEN=$(openssl rand -hex 32)

# Generate K256 private key
export K256_PRIVATE_KEY=$(openssl ecparam -name secp256k1 -genkey -noout -outform DER | tail -c +8 | head -c 32 | xxd -p -c 32)

# Set your domain and SMTP settings
export BASE_DOMAIN_NAME="pds.yourdomain.com"
export EMAIL_SERVER_HOST="smtp.example.com"
export EMAIL_SERVER_PORT="587"
export APP_ORG_NAME="Your Organization"
```

Or create a `.env` file in the project directory with these variables.

## Starting the Service

```bash
docker-compose up -d
```

## Account Management

```bash
# Create invite codes
docker exec bluesky-server pdsadmin create-invite-code

# Create admin account
docker exec bluesky-server pdsadmin account create

# View logs
docker logs bluesky-server
```
=======
bluesky - Self-hosted Docker Compose deployment
>>>>>>> 61ac142c75b6 (🗃️ Major updates 🗃️)

## 📋 Description

Bluesky is a containerized service deployed using Docker Compose. This setup provides a complete, production-ready deployment with proper security defaults, logging, and configuration management.

## 🚀 Services

This deployment includes the following services:

- **server**: Service container

## 📦 Installation

### Using curl

```shell
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/bluesky/main/docker-compose.yaml" | docker compose -f - up -d
```

### Using git

```shell
git clone "https://github.com/composemgr/bluesky" ~/.local/srv/docker/bluesky
cd ~/.local/srv/docker/bluesky
docker compose up -d
```

### Using composemgr

```shell
composemgr install bluesky
```

## 🔧 Configuration

### Directory Structure

The project follows a standardized rootfs layout:

```
.
├── docker-compose.yaml
└── rootfs/
    ├── config/          # Application configuration files
    ├── data/            # Application data and logs
```

### Environment Variables

Key environment variables (with defaults):

```shell
# Core Settings
TZ=America/New_York                    # Timezone
BASE_HOST_NAME=${HOSTNAME}             # Hostname for the service
BASE_DOMAIN_NAME=                      # Domain name (optional)

# Email/SMTP Settings
EMAIL_SERVER_HOST=172.17.0.1           # SMTP server address
EMAIL_SERVER_PORT=587                  # SMTP server port
EMAIL_SERVER_MAIL_FROM=no-reply@${BASE_DOMAIN_NAME:-${BASE_HOST_NAME}}
EMAIL_SERVER_FROM_ORG=bluesky   # From organization name
```

All variables have sane defaults and can be overridden via `.env` or `app.env` files.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:59096
- **Production**: Configure your reverse proxy to forward to port 59096

For production deployments, use a reverse proxy (nginx, traefik, caddy) to handle SSL/TLS.

## 📂 Volumes

Data persistence locations:

- `./rootfs/config/` - Application configuration
- `./rootfs/data/` - Application data and logs

## 🔐 Security

- All secrets use secure defaults with `changeme_*` prefix for easy identification
- No hardcoded passwords in compose file  
- Environment-based configuration for sensitive data
- Logging configured with rotation (5MB max, 1 file retained)
- SMTP configured for Docker gateway (172.17.0.1:587)

## 🔍 Logging

All services use standardized logging:
- **Driver**: json-file
- **Max Size**: 5MB per file
- **Max Files**: 1 (rotated)

View logs:
```shell
docker compose logs -f
docker compose logs -f [service_name]
```

## 🛠️ Management

### Start services
```shell
docker compose up -d
```

### Stop services
```shell
docker compose down
```

### Restart services
```shell
docker compose restart
```

### Update images
```shell
docker compose pull
docker compose up -d
```

### View status
```shell
docker compose ps
```

### Execute commands in container
```shell
docker compose exec [service_name] [command]
```

## 🔄 Backup & Restore

### Backup
```shell
# Backup volumes
tar -czf bluesky-backup-$(date +%Y%m%d).tar.gz rootfs/
```

### Restore
```shell
# Restore from backup
tar -xzf bluesky-backup-YYYYMMDD.tar.gz
docker compose up -d
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+
- Sufficient disk space for data and logs

## 🆘 Troubleshooting

### Check service status
```shell
docker compose ps
```

### View detailed logs
```shell
docker compose logs --tail=100 -f
```

### Restart a specific service
```shell
docker compose restart [service_name]
```

### Reset and start fresh
```shell
docker compose down -v
docker compose up -d
```

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
