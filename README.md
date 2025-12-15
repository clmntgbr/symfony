# Symfony Docker Template

A modern, production-ready [Symfony](https://symfony.com) application template powered by [Docker](https://www.docker.com/), [FrankenPHP](https://frankenphp.dev), and [Caddy](https://caddyserver.com/).

![CI](https://github.com/dunglas/symfony-docker/workflows/CI/badge.svg)

## 📋 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Development](#development)
- [Quality Assurance](#quality-assurance)
- [Production](#production)
- [Documentation](#documentation)
- [License](#license)

## 🎯 Overview

This project is a complete Symfony application template with Docker integration, designed for modern PHP development. It includes everything you need to build scalable web applications with built-in support for real-time features, database management, and message queuing.

### Key Features

- 🚀 **Blazing-fast performance** with [FrankenPHP worker mode](https://frankenphp.dev/docs/worker/)
- 🔒 **Automatic HTTPS** in development and production
- 🔄 **Real-time messaging** with built-in [Mercure hub](https://symfony.com/doc/current/mercure.html)
- 📦 **Complete Docker setup** with PostgreSQL and RabbitMQ
- 🧪 **Quality assurance tools** (PHPStan, PHP CS Fixer, Rector)
- 🔧 **HTTP/3** and [Early Hints](https://symfony.com/blog/new-in-symfony-6-3-early-hints) support
- 🐛 **Native XDebug** integration for debugging
- ⚙️ **CI/CD ready** with GitHub Actions

## 🛠 Tech Stack

### Core Framework
- **Symfony 8.0** - Latest stable version
- **PHP 8.4** - Modern PHP with latest features

### Infrastructure
- **FrankenPHP** - Modern PHP application server
- **Caddy** - Automatic HTTPS web server
- **PostgreSQL 16** - Robust relational database
- **RabbitMQ 3** - Message broker for asynchronous processing

### Symfony Bundles
- **Doctrine ORM 3.5** - Database abstraction and ORM
- **Doctrine Migrations** - Database versioning
- **Symfony Mercure** - Server-Sent Events (SSE) protocol
- **Symfony Mailer** - Email sending

### PHP Extensions Installed
- Core: `ctype`, `iconv`, `xml`, `sodium`
- Database: `pdo`, `pdo_mysql`, `pdo_pgsql`
- Performance: `opcache`, `apcu`, `igbinary`
- Image: `gd`, `imagick`
- Other: `intl`, `zip`, `sockets`, `amqp`

### Quality Assurance Tools
- **PHPStan 1.12** - Static analysis with Symfony integration
- **PHP CS Fixer 3.92** - Code style enforcement
- **Rector 1.2** - Automated refactoring and upgrades

## 🚀 Getting Started

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) 20.10+
- [Docker Compose](https://docs.docker.com/compose/install/) v2.10+

### Installation

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd symfony-docker
   ```

2. **Build Docker images**
   ```bash
   docker compose build --pull --no-cache
   ```

3. **(Optional) Configure environment**
   
   Create a `.env` file to override default values (see [Environment Variables](#environment-variables) section below).
   The application will work with defaults if no `.env` file is provided.

4. **Start the application**
   ```bash
   docker compose up --wait
   ```
   
   The `--wait` flag ensures all services are healthy before the command returns.

5. **Access the application**
   - HTTP: `http://localhost`
   - HTTPS: `https://localhost` (accept the self-signed certificate)
   - Mercure Hub: `https://localhost/.well-known/mercure`

6. **Stop the application**
   ```bash
   docker compose down --remove-orphans
   ```

## 💻 Development

### Environment Variables

Key environment variables (configure in `.env`):

```env
# Server
SERVER_NAME=localhost
HTTP_PORT=80
HTTPS_PORT=443

# Database
POSTGRES_DB=app
POSTGRES_USER=app
POSTGRES_PASSWORD=!ChangeMe!
POSTGRES_VERSION=16

# RabbitMQ
RABBITMQ_USER=guest
RABBITMQ_PASS=guest
RABBITMQ_VHOST=/

# Mercure
CADDY_MERCURE_JWT_SECRET=!ChangeThisMercureHubJWTSecretKey!
```

### Services & Ports

| Service    | Container | Port(s)          | Description                |
|------------|-----------|------------------|----------------------------|
| PHP/Caddy  | `php`     | 80, 443, 443/udp | Application server         |
| PostgreSQL | `db`      | 5432 (internal)  | Database                   |
| RabbitMQ   | `rmq`     | 9002, 9003       | Message broker & dashboard |

### Common Commands

**Access PHP container:**
```bash
docker compose exec php sh
```

**Run Symfony console:**
```bash
docker compose exec php bin/console [command]
```

**Install Composer dependencies:**
```bash
docker compose exec php composer install
```

**Database migrations:**
```bash
# Create migration
docker compose exec php bin/console make:migration

# Run migrations
docker compose exec php bin/console doctrine:migrations:migrate
```

**Clear cache:**
```bash
docker compose exec php bin/console cache:clear
```

### XDebug

XDebug is available in development mode. See [docs/xdebug.md](docs/xdebug.md) for configuration details.

## 🧪 Quality Assurance

This project includes three quality assurance tools that run automatically in CI/CD.

### PHPStan - Static Analysis

```bash
# Run analysis
composer phpstan

# Inside Docker
docker compose exec php composer phpstan
```

Configuration: `phpstan.neon.dist`

### PHP CS Fixer - Code Style

```bash
# Check code style
composer cs-check

# Fix code style
composer cs-fix

# Inside Docker
docker compose exec php composer cs-check
docker compose exec php composer cs-fix
```

Configuration: `.php-cs-fixer.dist.php`

### Rector - Automated Refactoring

```bash
# Check for refactoring opportunities
composer rector-check

# Apply refactoring
composer rector-fix

# Inside Docker
docker compose exec php composer rector-check
docker compose exec php composer rector-fix
```

Configuration: `rector.php`

### Run All QA Tools

```bash
# Run all checks at once
composer qa

# Inside Docker
docker compose exec php composer qa
```

### CI/CD

All quality tools run automatically in GitHub Actions on every push and pull request:
- ✅ Static analysis (PHPStan)
- ✅ Code style validation (PHP CS Fixer)
- ✅ Refactoring checks (Rector)
- ✅ HTTP/HTTPS reachability tests
- ✅ Mercure hub validation

See [`.github/workflows/ci.yaml`](.github/workflows/ci.yaml) for details.

## 🚢 Production

### Build Production Images

```bash
docker compose -f compose.yaml -f compose.prod.yaml build --pull --no-cache
```

### Deploy to Production

For detailed production deployment instructions, see [docs/production.md](docs/production.md).

Key production features:
- Optimized PHP configuration
- OPcache enabled
- Production-ready Caddy configuration
- Automatic HTTPS with Let's Encrypt
- HTTP/3 support

## 📚 Documentation

Detailed documentation is available in the `docs/` directory:

1. [Options available](docs/options.md)
2. [Using with an existing project](docs/existing-project.md)
3. [Extra services](docs/extra-services.md)
4. [Production deployment](docs/production.md)
5. [Debugging with XDebug](docs/xdebug.md)
6. [TLS certificates](docs/tls.md)
7. [Using MySQL instead of PostgreSQL](docs/mysql.md)
8. [Using Alpine Linux](docs/alpine.md)
9. [Makefile usage](docs/makefile.md)
10. [Updating the template](docs/updating.md)
11. [Quality assurance tools](docs/quality-tools.md)
12. [Troubleshooting](docs/troubleshooting.md)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run quality checks: `composer qa`
5. Fix any issues: `composer cs-fix && composer rector-fix`
6. Submit a pull request

## 📄 License

This project is available under the MIT License.

## 🙏 Credits

Based on [Symfony Docker](https://github.com/dunglas/symfony-docker) created by [Kévin Dunglas](https://dunglas.dev), co-maintained by [Maxime Helias](https://twitter.com/maxhelias) and sponsored by [Les-Tilleuls.coop](https://les-tilleuls.coop).
