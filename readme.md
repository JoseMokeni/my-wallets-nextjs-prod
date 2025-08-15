# My Wallets - Next.js Production Deployment

A personal expense tracking application built with Next.js, deployed using Docker and Traefik reverse proxy.

## Features

- Personal expense tracking and wallet management
- GitHub and Google OAuth authentication via Auth.js
- Email notifications via Resend
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
- `NEXTAUTH_URL`: Your application's full URL (e.g., "https://mywallets.josemokeni.cloud")
- `AUTH_GITHUB_ID` & `AUTH_GITHUB_SECRET`: From GitHub OAuth App
- `AUTH_GOOGLE_ID` & `AUTH_GOOGLE_SECRET`: From Google OAuth App
- `DATABASE_URL`: PostgreSQL connection string
- `RESEND_API_KEY`: API key from Resend for email functionality
- `RESEND_FROM_NAME`: Display name for email sender (e.g., "My Wallets")
- `RESEND_FROM_EMAIL`: Email address for sending emails (e.g., "do-not-reply@mywallets.com")

### 3. Email Service Setup (Resend)

1. Sign up at [Resend](https://resend.com)
2. Create an API key in your Resend dashboard
3. Add the API key to your `.env` file as `RESEND_API_KEY`
4. Configure your domain for sending emails (optional, for custom from addresses)
5. Set `RESEND_FROM_NAME` for the sender display name
6. Set `RESEND_FROM_EMAIL` with your verified domain email or use Resend's default

### 4. Database Setup

The PostgreSQL database will be created automatically via Docker Compose. To set up the database schema:

```bash
# Push the Prisma schema to the database
docker compose exec app-prod npx prisma db push

# Optional: Generate Prisma client (if needed)
docker compose exec app-prod npx prisma generate
```

### 5. Deploy

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
docker compose exec app-prod npx prisma db push --skip-generate
```

### Seed Database

```bash
docker compose exec app-prod npx prisma db seed
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
4. **Google OAuth**: Verify Google OAuth App settings and authorized domains
5. **Email Issues**: Check Resend API key validity and domain configuration

## Network Architecture

```
Internet → Traefik (Port 80/443) → App Container (Port 3000)
                                  ↓
                              PostgreSQL (Port 5432)
```

## License

Private project - All rights reserved.
