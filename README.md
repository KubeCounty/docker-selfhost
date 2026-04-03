# docker-selfhost

**KubeCounty** — Self-hosted, open-source alternatives to popular software. One `docker compose up -d` away.

No vendor lock-in. No SaaS bills. Full control over your data.

---

## Why This Exists

Most software you pay for monthly has a perfectly good open-source alternative. The barrier isn't the software — it's the setup. This repo removes that barrier. Every stack here is:

- **One command to run** — `docker compose up -d` and it works
- **Production-minded** — persistent volumes, health checks, restart policies, isolated networks
- **Documented** — env vars explained, backup commands included, update instructions clear
- **Self-contained** — no dependencies outside Docker and a `.env` file

---

## Stacks

### CMS & Websites
| Stack | Replaces | Folder |
|-------|----------|--------|
| WordPress + MariaDB | Squarespace, Wix, Webflow | [`wordpress/`](./wordpress/) |

### Coming Soon
| Stack | Replaces |
|-------|----------|
| Ghost | Substack, Beehiiv |
| Plausible Analytics | Google Analytics |
| Directus | Contentful, Sanity |
| Gitea | GitHub (private repos) |
| Nextcloud | Google Drive, Dropbox |
| Umami | Google Analytics |
| Pocketbase | Firebase |
| Mattermost | Slack |
| Formbricks | Typeform, SurveyMonkey |
| Metabase | Tableau, Looker |
| n8n | Zapier, Make |
| Appwrite | Firebase, Supabase |
| Supabase | Firebase |

> Want a stack added? [Open an issue](https://github.com/kubecounty/docker-selfhost/issues).

---

## Quick Start

### Prerequisites
- Docker Engine 24+
- Docker Compose v2 (`docker compose` not `docker-compose`)

### Run any stack

```bash
# Clone the repo
git clone https://github.com/kubecounty/docker-selfhost.git
cd docker-selfhost

# Pick a stack
cd wordpress

# Copy and fill in the env file
cp .env.example .env
nano .env

# Start
docker compose up -d
```

---

## Repo Structure

```
docker-selfhost/
├── wordpress/
│   ├── docker-compose.yml
│   ├── .env.example
│   └── README.md
├── ghost/               # coming soon
├── plausible/           # coming soon
└── ...
```

Each folder contains:
- `docker-compose.yml` — the full stack definition
- `.env.example` — all required environment variables with descriptions
- `README.md` — setup instructions, backup commands, update steps

---

## Conventions

Every stack in this repo follows the same patterns so you only have to learn them once.

**Persistent data** — all stateful data lives in named Docker volumes. Your data survives container rebuilds and image updates.

**Health checks** — every service has a health check. Dependent services wait for their dependencies to be healthy before starting.

**Isolated networks** — databases are never exposed to the host. Only the application layer exposes ports.

**Environment variables** — all configuration lives in `.env`. The `docker-compose.yml` never contains hardcoded secrets.

**Restart policy** — all services use `restart: unless-stopped`. Your stack comes back up after a server reboot without manual intervention.

---

## Updating a Stack

```bash
# Pull latest images
docker compose pull

# Recreate containers with new images (zero config change)
docker compose up -d
```

---

## Backups

Each stack's `README.md` includes the exact backup command for that stack. General pattern for database backups:

```bash
# MySQL / MariaDB
docker exec <db-container> sh -c \
  'mysqldump -u root -p"$MYSQL_ROOT_PASSWORD" <dbname>' \
  > backup_$(date +%Y%m%d).sql

# PostgreSQL
docker exec <db-container> pg_dump -U <user> <dbname> \
  > backup_$(date +%Y%m%d).sql
```

---

## Contributing

PRs welcome. To add a new stack:

1. Create a folder named after the primary application
2. Include `docker-compose.yml`, `.env.example`, and `README.md`
3. Follow the conventions above — persistent volumes, health checks, isolated network, no hardcoded secrets
4. Test it from scratch: `docker compose up -d` should work after filling in `.env.example`

---

## Kubernetes Version

Running these at scale? The Kubernetes equivalents of these stacks live in [`kubecounty/k8s-selfhost`](https://github.com/kubecounty/k8s-selfhost) — coming soon.

---

*Built by [KubeCounty](https://tiktok.com/@kubecounty) — Kubernetes Infrastructure Consulting*  
*Found this useful? Follow on [TikTok](https://tiktok.com/@kubecounty) or [LinkedIn](https://linkedin.com/company/kubecounty)*
