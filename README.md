## 👋 Welcome to bluesky 🚀

Self-hosted Bluesky (AT Protocol) Personal Data Server

## 📋 Description

Runs the official Bluesky Personal Data Server (PDS). Accounts are
invite-only and require e-mail verification. The PDS stores account data and
blobs under `./volumes/data/bluesky`.

## 🚀 Services

- **server**: ghcr.io/bluesky-social/pds:latest

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/bluesky/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/bluesky" ~/.local/srv/docker/bluesky
cd ~/.local/srv/docker/bluesky
cp default.env .env
cat app.env.sample >> .env
docker compose --env-file .env up -d
```

The PDS refuses to start without `APP_ADMIN_PASS`, `APP_JWT_TOKEN` and
`K256_PRIVATE_KEY`, so set them in `.env` (or `app.env` for composemgr) first.

### Option 3: Using composemgr
```bash
composemgr install bluesky
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ORG_NAME=MyOrg
APP_ADMIN_PASS=replace-with-a-random-password
APP_JWT_TOKEN=replace-with-a-random-32-character-hex-string
K256_PRIVATE_KEY=replace-with-a-random-64-character-hex-string
PUBLIC_IP=
EMAIL_SERVER_HOST=172.17.0.1
EMAIL_SERVER_PORT=587
EMAIL_SERVER_MAIL_FROM=no-reply@example.com
```

Generate the required secrets with:

```bash
openssl rand -base64 32
openssl rand -hex 32
openssl ecparam -name secp256k1 -genkey -noout -outform DER | tail -c +8 | head -c 32 | xxd -p -c 32
```

See `docker-compose.yaml` for the complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64122` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The proxy variants publish no host ports; the app is reached only through the
proxy network.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64122

The PDS must be reachable over HTTPS on `BASE_HOST_NAME`; terminate TLS in your
reverse proxy.

Create invite codes through the PDS admin API, authenticating as `admin` with
`APP_ADMIN_PASS`:

```bash
curl -X POST "https://bluesky.example.com/xrpc/com.atproto.server.createInviteCode" \
  -u "admin:${APP_ADMIN_PASS}" -H "Content-Type: application/json" \
  -d '{"useCount": 1}'
```

## 📂 Volumes

- `./volumes/data/bluesky` - PDS account data, repositories, and blobs

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly
- Registration is invite-only and e-mail verification is required
- Keep `K256_PRIVATE_KEY` secret and backed up: it is the PLC rotation key

## 🔍 Logging

```shell
docker compose --env-file .env logs -f server
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
