# Onboarding Guide — DevOps Engineer

**Team:** DevOps  
**Role Overview:** Build and maintain the infrastructure, containerization, CI/CD pipeline, and production environment that keeps the application running reliably on the target cloud platform.

---

## 1. Welcome

You are responsible for everything between code and the end user. Your work enables the rest of the team to ship confidently — knowing that their code will be automatically tested, containerized, deployed, and monitored without manual intervention. You own the reliability, performance, and security of the production environment.

The application runs on **[DEPLOYMENT_TARGET]** (see Project Config Sheet). The examples below use **GCP Cloud Run** as a reference. You will work with **Docker**, **GitHub Actions**, **Terraform** (for infrastructure as code), and the full suite of cloud services for your target platform. Claude Desktop is your infrastructure advisor and automation assistant.

---

## 2. Your Responsibilities

**Containerization**
- Author and maintain the `Dockerfile` for the application
- Optimize Docker images for fast build times and minimal image size
- Ensure containers start correctly, handle signals properly, and run as non-root users

**CI/CD Pipeline**
- Build and maintain GitHub Actions workflows for test, build, and deploy
- Ensure every push to `develop` is automatically deployed to the development environment
- Ensure every merge to `main` triggers a production deployment after all checks pass
- Maintain deployment speed — target total pipeline time under 8 minutes

**GCP Infrastructure**
- Provision and manage all GCP services used by the application (Cloud Run, Cloud SQL, Memorystore, Artifact Registry, Secret Manager, Cloud Storage)
- Write Terraform configurations for reproducible infrastructure provisioning
- Manage GCP IAM — least-privilege roles for services and team members
- Set up and maintain VPC networking, Cloud Armor (WAF), and Cloud Load Balancing if required

**Secrets & Configuration Management**
- Store all application secrets in GCP Secret Manager
- Ensure the application pulls secrets at runtime — never bake them into images
- Manage environment-specific configuration (development, staging, production)

**Monitoring & Observability**
- Configure Cloud Logging to capture all application and server logs
- Set up Cloud Monitoring dashboards for CPU, memory, request latency, and error rate
- Create alerting policies that notify the team of anomalies (error spikes, high latency, failed deployments)
- Document runbooks for common incident responses in `by-team/devops/infra-notes.md`

**Security**
- Ensure the production environment passes basic security hardening (no public database, encrypted at rest and in transit, secrets never in code)
- Conduct periodic review of GCP IAM permissions — remove unused service accounts and roles

---

## 3. GCP Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  GitHub Actions CI/CD                                        │
│  (build → test → push image → deploy)                        │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  GCP Artifact Registry                                       │
│  [REGION]-docker.pkg.dev/[GCP_PROJECT]/[project-code]/[project-code]:tag │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  GCP Cloud Run (us-central1)                                 │
│  • Min instances: 1 (prod) / 0 (dev, staging)               │
│  • Max instances: 10                                         │
│  • CPU: 2 vCPU, Memory: 1 GiB                               │
│  • Concurrency: 80                                           │
└──────┬──────────────────┬──────────────────┬────────────────┘
       │                  │                  │
       ▼                  ▼                  ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐
│ Cloud SQL    │  │ Memorystore  │  │ Cloud Storage         │
│ PostgreSQL   │  │ Redis 7      │  │ (file uploads, logs)  │
│ (private IP) │  │ (private IP) │  │                       │
└──────────────┘  └──────────────┘  └──────────────────────┘
       │
       ▼
┌──────────────┐
│ Secret Mgr   │
│ (all secrets │
│ at runtime)  │
└──────────────┘
```

---

## 4. GCP Services Reference

| Service | Purpose | Notes |
|---|---|---|
| **Cloud Run** | Host the application Docker container | Stateless; sessions/cache go to Redis |
| **Cloud SQL (PostgreSQL 16)** | Primary database | Private IP only; no public access |
| **Memorystore (Redis 7)** | Cache, session, queue | Private IP only |
| **Artifact Registry** | Docker image registry | Replace Container Registry (deprecated) |
| **Secret Manager** | Store all env secrets | Mount as volume or env at runtime |
| **Cloud Storage** | File uploads and reports | CORS configured for app domain |
| **Cloud Logging** | Structured log aggregation | Use JSON logging (how-to depends on your framework) |
| **Cloud Monitoring** | Metrics, dashboards, alerts | Set up SLO-based alerts |
| **Cloud Build** | Optional: fast builds on GCP infra | Used if GitHub Actions is too slow |
| **VPC** | Private networking for SQL and Redis | Cloud Run connects via Serverless VPC Access |

---

## 5. Dockerfile

The Dockerfile lives in the root of the repository.

> **Note:** The example below is for a PHP/Laravel project. Adapt the base image and build commands for your project's runtime (e.g., Node.js, Python, Go).

```dockerfile
# Example for a PHP/Laravel project — adapt for your stack

# Build stage
FROM php:8.3-fpm-alpine AS build

WORKDIR /app

# System dependencies
RUN apk add --no-cache \
    postgresql-dev \
    libzip-dev \
    zip \
    unzip \
    nodejs \
    npm

# PHP extensions
RUN docker-php-ext-install \
    pdo \
    pdo_pgsql \
    zip \
    opcache

# Composer (PHP dependency manager — substitute with your project's package manager)
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Install PHP dependencies (production only)
COPY composer.json composer.lock ./
RUN composer install --no-dev --optimize-autoloader --no-interaction

# Install & build Node assets
COPY package.json package-lock.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Production stage
FROM php:8.3-fpm-alpine AS production

WORKDIR /app

RUN apk add --no-cache postgresql-dev nginx supervisor

RUN docker-php-ext-install pdo pdo_pgsql opcache

# Copy from build stage
COPY --from=build /app /app

# Nginx and supervisor config
COPY docker/nginx.conf /etc/nginx/nginx.conf
COPY docker/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# OPcache config
COPY docker/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# File permissions
RUN chown -R www-data:www-data /app/storage /app/bootstrap/cache

# Cloud Run listens on 8080
EXPOSE 8080

# Run as non-root
USER www-data

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
```

---

## 6. GitHub Actions CI/CD Workflow

> **Note:** The `composer install`, `php artisan key:generate`, `php artisan migrate`, and `php artisan test` commands below are Laravel-specific. Substitute with your project's equivalent commands (e.g., `npm install` / `npm test`, `pip install` / `pytest`, etc.).

Create `.github/workflows/deploy.yml`:

```yaml
name: Build, Test & Deploy

on:
  push:
    branches: [develop, main]
  pull_request:
    branches: [develop]

env:
  PROJECT_ID: qtrust-[project-code]
  REGION: us-central1
  SERVICE_NAME: [project-code]-app
  REGISTRY: [REGION]-docker.pkg.dev/[GCP_PROJECT]/[project-code]/[project-code]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_DB: [project-code]_test
          POSTGRES_USER: [project-code]_user
          POSTGRES_PASSWORD: secret
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
    steps:
      - uses: actions/checkout@v4
      - uses: shivammathur/setup-php@v2
        with:
          php-version: '8.3'
          extensions: pdo_pgsql
      # Laravel-specific commands below — substitute for your project's framework:
      - run: composer install --no-interaction
      - run: cp .env.example .env.testing && php artisan key:generate --env=testing
      - run: php artisan migrate --env=testing
      - run: php artisan test --env=testing

  deploy:
    needs: test
    if: github.ref == 'refs/heads/develop' || github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: google-github-actions/auth@v2
        with:
          credentials_json: ${{ secrets.GCP_SA_KEY }}
      - uses: google-github-actions/setup-gcloud@v2
      - run: gcloud auth configure-docker ${{ env.REGION }}-docker.pkg.dev

      - name: Build and push Docker image
        run: |
          IMAGE="${{ env.REGISTRY }}:${{ github.sha }}"
          docker build -t $IMAGE .
          docker push $IMAGE

      - name: Deploy to Cloud Run
        run: |
          ENVIRONMENT=$([[ "${{ github.ref }}" == "refs/heads/main" ]] && echo "production" || echo "development")
          gcloud run deploy ${{ env.SERVICE_NAME }}-${ENVIRONMENT} \
            --image ${{ env.REGISTRY }}:${{ github.sha }} \
            --region ${{ env.REGION }} \
            --platform managed \
            --allow-unauthenticated \
            --set-secrets="APP_KEY=[project-code]-app-key:latest,DB_PASSWORD=[project-code]-db-password:latest" \
            --update-env-vars="APP_ENV=${ENVIRONMENT}"
```

---

## 7. Environment Variables & Secrets

**Never store secrets in:**
- `.env` files committed to the repository
- Docker images
- GitHub Actions environment variables (unless using GitHub Secrets for non-sensitive config)
- Google Drive or any shared document

**All secrets go into GCP Secret Manager.** Access them from Cloud Run via `--set-secrets` flag in the deployment command.

### Required Secrets in Secret Manager

| Secret Name | Description |
|---|---|
| `[project-code]-app-key` | Application secret key |
| `[project-code]-db-host` | Cloud SQL private IP |
| `[project-code]-db-name` | Database name |
| `[project-code]-db-user` | Database user |
| `[project-code]-db-password` | Database password |
| `[project-code]-redis-host` | Memorystore Redis host |
| `[project-code]-storage-bucket` | Cloud Storage bucket name |
| `[project-code]-mail-password` | SMTP or mail API key |

### Non-sensitive configuration
Store in Cloud Run environment variables (visible in GCP Console, not sensitive):
```
APP_ENV=production
APP_DEBUG=false
APP_URL=https://[project-code].qtrust.id
CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
SESSION_DRIVER=redis
LOG_CHANNEL=stderr
LOG_LEVEL=warning
```

---

## 8. Monitoring Setup

### Cloud Monitoring Alerts to Configure
| Alert | Condition | Notification |
|---|---|---|
| High error rate | HTTP 5xx > 1% over 5 min | Email + Slack |
| High latency | p95 response time > 2s over 5 min | Email |
| Low container count | Instances < 1 (production) | Email + Slack |
| Failed deployments | Cloud Run deployment failure | Email + Slack |
| Database CPU | Cloud SQL CPU > 80% for 10 min | Email |

### Structured Logging
Configure the application to output JSON logs to stderr (Cloud Run captures stderr automatically). Use JSON logging — how-to depends on your framework:

```php
// Example for Laravel — config/logging.php
'channels' => [
    'stderr' => [
        'driver'    => 'monolog',
        'handler'   => StreamHandler::class,
        'formatter' => JsonFormatter::class,
        'with'      => ['stream' => 'php://stderr'],
        'level'     => env('LOG_LEVEL', 'warning'),
    ],
],
```

---

## 9. Terraform (Infrastructure as Code)

Store Terraform files in the repository at `infrastructure/terraform/`.

```hcl
# infrastructure/terraform/main.tf
provider "google" {
  project = "[GCP_PROJECT]"
  region  = "us-central1"
}

resource "google_cloud_run_v2_service" "app" {
  name     = "[project-code]-app-production"
  location = "us-central1"

  template {
    containers {
      image = "[REGION]-docker.pkg.dev/[GCP_PROJECT]/[project-code]/[project-code]:latest"
      resources {
        limits = { cpu = "2", memory = "1Gi" }
      }
    }
    scaling {
      min_instance_count = 1
      max_instance_count = 10
    }
  }
}
```

Run:
```bash
terraform init
terraform plan
terraform apply
```

---

## 10. Working with Claude Desktop

**Generate a Dockerfile:**
> "Example for Laravel — adapt for your stack: Create a production-optimized multi-stage Dockerfile for a Laravel 13 application. It should use PHP 8.3 FPM Alpine, install PostgreSQL extension, run Vite build, use Nginx as the web server, and Supervisor to manage PHP-FPM and Nginx processes. The container must listen on port 8080 for Cloud Run compatibility and run as a non-root user."

**Generate a GitHub Actions workflow:**
> "Example for Laravel — adapt for your stack: Write a GitHub Actions CI/CD workflow for a Laravel 13 app that: runs Pest tests against a PostgreSQL 16 service container, builds a Docker image, pushes to GCP Artifact Registry, and deploys to GCP Cloud Run. Use Workload Identity Federation for authentication instead of a service account key."

**Generate Terraform config:**
> "Write Terraform configuration to provision: a GCP Cloud Run service, a Cloud SQL PostgreSQL 16 instance with private IP, a Memorystore Redis instance, and the required VPC Serverless Access Connector. All in us-central1."

**Incident runbook:**
> "Write an incident runbook for a scenario where the [project-code] [service-name] service is returning HTTP 503 errors. Include: diagnostic steps, common causes, resolution steps, and how to roll back to the previous deployment."

---

## 11. Deployment Checklist (Pre-Production)

Before every production deployment, verify:
- [ ] All automated tests pass in CI
- [ ] Docker image builds successfully and starts without errors locally
- [ ] All secrets in GCP Secret Manager are up to date
- [ ] Database migrations have been reviewed — no destructive operations without a rollback plan
- [ ] Cloud SQL backups are recent (within 24 hours)
- [ ] Staging deployment tested and QA sign-off obtained
- [ ] Rollback plan documented — previous image tag noted
- [ ] Team notified of deployment window

---

## 12. First Week Checklist

- [ ] GCP project access granted — verify you can access `qtrust-[project-code]` project
- [ ] GCP IAM role assigned: `roles/run.admin`, `roles/cloudsql.admin`, `roles/secretmanager.admin`
- [ ] GitHub Actions secrets configured: `GCP_SA_KEY` (or Workload Identity set up)
- [ ] Local Docker environment tested: `docker build` and `docker run` work
- [ ] GCP CLI installed: `gcloud auth login` works
- [ ] Terraform installed and initialized against the GCP project
- [ ] Claude Desktop installed and workspace folder connected
- [ ] `by-team/devops/infra-notes.md` reviewed
- [ ] Existing CI/CD pipeline reviewed and understood
- [ ] First deployment to development environment completed

---

## 13. Key Resources

| Resource | Location |
|---|---|
| Project Configuration Sheet | Provided by your team lead |
| Infra Notes | `by-team/devops/infra-notes.md` |
| Environment Variables List | `by-team/devops/env-variables.md` |
| Deployment Checklist | `by-team/devops/deployment-checklist.md` |
| Incident Log | `by-team/devops/incident-log.md` |
| GCP Console | [console.cloud.google.com](https://console.cloud.google.com) |
| Cloud Run Docs | [cloud.google.com/run/docs](https://cloud.google.com/run/docs) |
| Cloud SQL Docs | [cloud.google.com/sql/docs](https://cloud.google.com/sql/docs) |
| GitHub Actions Docs | [docs.github.com/actions](https://docs.github.com/en/actions) |
| Terraform GCP Provider | [registry.terraform.io/providers/hashicorp/google](https://registry.terraform.io/providers/hashicorp/google/latest/docs) |
| Docker Best Practices | [docs.docker.com/develop/develop-images/dockerfile_best-practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) |

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · GitHub (write) · Google Drive · Slack · Sentry (admin) · Datadog (admin) · Linear (read)

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Sentry | [`tools/sentry.md`](../tools/sentry.md) | Before first staging deploy |
| Datadog | [`tools/datadog.md`](../tools/datadog.md) | Before infra provisioning |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
