# UGREEN NAS Syncthing Reporter

[Deutsch](README.DE.md)

![SyncthingReporter](Screens/SyncthingReporter.png)

UGREEN NAS Syncthing Reporter is a lightweight Docker package for Syncthing. It generates a daily HTML report and can deliver it via SMTP or Apprise.

The package supports English and German through `REPORT_LANG=en` or `REPORT_LANG=de`. It is particularly well suited to UGREEN NAS systems running UGOS, but it also works on other Docker hosts.

## Features

- Daily HTML report for Syncthing
- Delivery via SMTP or Apprise
- English and German runtime language support
- Overview of folder status, API errors, and failed items
- Evaluation of changes from the last X hours using `WINDOW_HOURS`
- Outlook-friendly HTML layout
- Prebuilt Docker image from GitHub Container Registry
- Local build option for development and custom builds
- GitHub Actions workflow for automated image builds and Trivy scans
- Dependabot configuration for the Dockerfile, Python dependencies, and GitHub Actions

## Docker image

By default, the Compose file uses this image:

```text
ghcr.io/railsimulatornet/ugreen-nas-syncthing-reporter:latest
```

This allows the reporter to be updated with `docker compose pull` or through the UGOS Docker app whenever a new image is published.

## Repository structure

```text
UGREEN-NAS-Syncthing-Reporter/
├─ README.md
├─ README.DE.md
├─ CHANGELOG.md
├─ LICENSE
├─ .gitignore
├─ .github/
│  ├─ dependabot.yml
│  └─ workflows/
│     └─ docker-publish.yml
├─ Screens/
│  ├─ DE_Mail.jpg
│  ├─ DE_MailMobil.jpg
│  └─ SyncthingReporter.png
├─ UGREEN_Syncthing_Reporter_Handbuch_DE-EN.pdf
└─ syncthing/
   ├─ .env.example
   ├─ docker-compose.yaml
   ├─ docker-compose.local-build.yaml
   ├─ scripts/
   │  ├─ update_reporter.sh
   │  ├─ rebuild_reporter_local.sh
   │  └─ security_scan_reporter.sh
   ├─ syncthing/
   │  └─ config/
   │     └─ PLACEHOLDER.txt
   └─ syncthing_reporter_py/
      ├─ .dockerignore
      ├─ Dockerfile
      ├─ entry.sh
      ├─ report.py
      ├─ requirements.txt
      └─ scheduler.sh
```

## Quick start

1. Copy the `syncthing` directory to your NAS or Docker host.
2. Copy `syncthing/.env.example` to `syncthing/.env`.
3. Adjust the values in `.env` for your environment.
4. Add your own Syncthing data paths to `docker-compose.yaml` if required.
5. Start the stack:

```bash
cd syncthing
docker compose pull
docker compose up -d
```

## Updating

```bash
cd syncthing
docker compose pull
docker compose up -d
```

On UGOS, you can alternatively redeploy the project in the Docker app and make sure the latest image is pulled.

## Local build

For local testing or customized builds, use the local-build Compose file:

```bash
cd syncthing
docker compose -f docker-compose.local-build.yaml build --pull --no-cache syncthing_reporter_py
docker compose -f docker-compose.local-build.yaml up -d
```

Alternatively:

```bash
cd syncthing
chmod +x scripts/rebuild_reporter_local.sh
./scripts/rebuild_reporter_local.sh 2.2.0
```

## Reporter image security scan

```bash
cd syncthing
chmod +x scripts/security_scan_reporter.sh
./scripts/security_scan_reporter.sh
```

Results are stored under `syncthing_reporter_py/state/security/`.

## GitHub Container Registry

The Docker image is built by GitHub Actions and published to GitHub Container Registry. The workflow runs when reporter code changes, for version tags, manually, and on a weekly schedule.

## Configuration

The main runtime settings are stored in `syncthing/.env`. Important options include:

- `REPORT_LANG=en|de` — report language; English is the default
- `SYNCTHING_URL` — Syncthing API URL
- `ST_API_KEY` — optional API key; when empty, the reporter can read it from the mounted `config.xml`
- `RUN_AT` — daily report execution time
- `WINDOW_HOURS` — reporting window for file changes
- `REPORT_HOSTNAME` — display name used in the report and subject
- `APPRISE_ENABLED`, `APPRISE_URL`, `APPRISE_URLS` — optional Apprise delivery
- `SMTP_*` — SMTP fallback settings

See `syncthing/.env.example` for the full set of options.

## License

This project is distributed under the **MIT License**.

## Documentation

The detailed bilingual manual is included in the repository as:

```text
UGREEN_Syncthing_Reporter_Handbuch_DE-EN.pdf
```

## Version

- Reporter version: V2.2
- Docker image: `ghcr.io/railsimulatornet/ugreen-nas-syncthing-reporter:latest`
- Package build date: 2026-05-21
