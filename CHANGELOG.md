# Changelog

## v2.2 - 2026-05-21

### Added

- Added a prebuilt Docker image published through GitHub Container Registry.
- Added a weekly scheduled GitHub Actions rebuild for the reporter image.
- Added Trivy image scanning in GitHub Actions.
- Added Dependabot configuration for GitHub Actions, the Dockerfile, and Python dependencies.
- Added `docker-compose.local-build.yaml` for local builds.
- Added `scripts/update_reporter.sh` for registry-based updates.
- Added `scripts/rebuild_reporter_local.sh` for local rebuilds.
- Added `scripts/security_scan_reporter.sh` for local Trivy scans of the reporter image.
- Added `.dockerignore` for a clean Docker build context.
- Updated `.env.example` for GHCR image usage.

### Changed

- The default `docker-compose.yaml` now uses `ghcr.io/railsimulatornet/ugreen-nas-syncthing-reporter:latest` instead of a local build.
- Changed the Dockerfile base image.
- Operating-system packages are upgraded during the image build.
- Removed unnecessary packages from the reporter image.
- Python dependency `requests` is constrained to the supported version range.
- Reporter metadata updated to V2.2 / 2026-05-21.
- English is now the default runtime language; German remains available with `REPORT_LANG=de`.
- Repository documentation, configuration comments, and source comments are now maintained in English.

### Notes

- Existing users can update by replacing the Compose file and running `docker compose pull && docker compose up -d`.
- Local builds remain available through `docker-compose.local-build.yaml`.
- The bilingual runtime report remains supported through `REPORT_LANG=en` and `REPORT_LANG=de`.
