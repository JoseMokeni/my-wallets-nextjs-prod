# My Wallets - Next.js Production Deployment

A personal expense tracking application built with Next.js, deployed using Docker and Traefik reverse proxy.

## Features

- Personal expense tracking and wallet management
- GitHub OAuth authentication via Auth.js
- PostgreSQL database with Prisma ORM
- Production-ready Docker deployment
- Automatic HTTPS with Let's Encrypt via Traefik

## Prerequisites

- Docker and Docker Compose
- Domain name configured to point to your server
- Traefik reverse proxy running (with `traefik-public` network)

## Setup Instructions

### 1. Clone and Configure

```bash
git clone <repository-url>
cd my-wallets-nextjs-prod
```

### 2. Environment Configuration

Copy the example environment file and configure your variables:

```bash
cp .env.example .env
```

Edit `.env` with your actual values:

- `AUTH_SECRET`: Generate with `npx auth secret`
- `AUTH_GITHUB_ID` & `AUTH_GITHUB_SECRET`: From GitHub OAuth App
- `DATABASE_URL`: PostgreSQL connection string

### 3. Database Setup

The PostgreSQL database will be created automatically via Docker Compose. To set up the database schema:

```bash
# Push the Prisma schema to the database
docker compose exec app-prod npx prisma db push

# Optional: Generate Prisma client (if needed)
docker compose exec app-prod npx prisma generate
```

### 4. Deploy

Start the application stack:

```bash
docker compose up -d
```

Check the logs:

```bash
docker compose logs -f
```

## Services

- **app-prod**: Next.js application container
- **postgres**: PostgreSQL database
- **Traefik**: Reverse proxy with automatic HTTPS (external)

## Access

The application will be available at: `https://mywallets.josemokeni.cloud`

## Database Management

### Push Schema Changes

```bash
docker compose exec app-prod npx prisma db push
```

### Access Database

```bash
docker compose exec postgres psql -U mywallets -d expense_tracker
```

### View Database in Prisma Studio

```bash
docker compose exec app-prod npx prisma studio
```

## Maintenance

### Update Application

```bash
docker compose pull app-prod
docker compose up -d app-prod
```

### Backup Database

```bash
docker compose exec postgres pg_dump -U mywallets expense_tracker > backup.sql
```

### View Logs

```bash
# All services
docker compose logs -f

# Specific service
docker compose logs -f app-prod
```

## Troubleshooting

1. **SSL Certificate Issues**: Ensure your domain points to the server and Traefik is running
2. **Database Connection**: Check if PostgreSQL container is running and environment variables are correct
3. **GitHub OAuth**: Verify GitHub App settings and callback URLs

## Network Architecture

```
Internet → Traefik (Port 80/443) → App Container (Port 3000)
                                  ↓
                              PostgreSQL (Port 5432)
```

## License

Private project - All rights reserved.
