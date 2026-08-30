# UGREEN NAS Syncthing Reporter

[English](README.md) · [Deutsch](README.DE.md)

![SyncthingReporter](Screens/SyncthingReporter.png)

UGREEN NAS Syncthing Reporter to lekki pakiet Docker dla Syncthing. Generuje codzienny raport HTML i może wysyłać go przez SMTP lub Apprise.

Pakiet obsługuje język angielski i niemiecki przez `REPORT_LANG=en` lub `REPORT_LANG=de`. Jest szczególnie dobrze dopasowany do systemów UGREEN NAS z UGOS, ale działa również na innych hostach Docker.

## Funkcje

- Codzienny raport HTML dla Syncthing
- Wysyłka przez SMTP lub Apprise
- Obsługa języka angielskiego i niemieckiego podczas działania
- Przegląd stanu folderów, błędów API i nieudanych elementów
- Analiza zmian z ostatnich X godzin za pomocą `WINDOW_HOURS`
- Układ HTML przyjazny dla Outlooka
- Gotowy obraz Docker publikowany w GitHub Container Registry
- Opcja lokalnego builda do testów i własnych modyfikacji
- Workflow GitHub Actions do automatycznego budowania obrazu i skanowania Trivy
- Konfiguracja Dependabot dla Dockerfile, zależności Pythona i GitHub Actions

## Obraz Docker

Domyślnie plik Compose używa obrazu:

```text
ghcr.io/railsimulatornet/ugreen-nas-syncthing-reporter:latest
```

Dzięki temu reporter może być aktualizowany poleceniem `docker compose pull` albo przez aplikację Docker w UGOS po opublikowaniu nowego obrazu.

## Struktura repozytorium

```text
UGREEN-NAS-Syncthing-Reporter/
├─ README.md
├─ README.DE.md
├─ README.PL.md
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

## Szybki start

1. Skopiuj katalog `syncthing` na NAS lub host Docker.
2. Skopiuj `syncthing/.env.example` do `syncthing/.env`.
3. Dostosuj wartości w `.env` do swojego środowiska.
4. W razie potrzeby dodaj własne ścieżki danych Syncthing w `docker-compose.yaml`.
5. Uruchom stack:

```bash
cd syncthing
docker compose pull
docker compose up -d
```

## Aktualizacja

```bash
cd syncthing
docker compose pull
docker compose up -d
```

W UGOS możesz alternatywnie ponownie wdrożyć projekt w aplikacji Docker i upewnić się, że pobierany jest najnowszy obraz.

## Lokalny build

Do testów lokalnych lub własnych buildów użyj pliku Compose dla lokalnego builda:

```bash
cd syncthing
docker compose -f docker-compose.local-build.yaml build --pull --no-cache syncthing_reporter_py
docker compose -f docker-compose.local-build.yaml up -d
```

Alternatywnie:

```bash
cd syncthing
chmod +x scripts/rebuild_reporter_local.sh
./scripts/rebuild_reporter_local.sh 2.2.0
```

## Skan bezpieczeństwa obrazu reportera

```bash
cd syncthing
chmod +x scripts/security_scan_reporter.sh
./scripts/security_scan_reporter.sh
```

Wyniki są zapisywane w `syncthing_reporter_py/state/security/`.

## GitHub Container Registry

Obraz Docker jest budowany przez GitHub Actions i publikowany w GitHub Container Registry. Workflow uruchamia się po zmianach w kodzie reportera, dla tagów wersji, ręcznie oraz cyklicznie raz w tygodniu.

## Konfiguracja

Główne ustawienia runtime znajdują się w `syncthing/.env`. Najważniejsze opcje:

- `REPORT_LANG=en|de` — język raportu; domyślnie angielski
- `SYNCTHING_URL` — adres API Syncthing
- `ST_API_KEY` — opcjonalny klucz API; jeśli pole jest puste, reporter może odczytać go z zamontowanego `config.xml`
- `RUN_AT` — godzina codziennego uruchomienia raportu
- `WINDOW_HOURS` — zakres czasu analizowanych zmian plików
- `REPORT_HOSTNAME` — nazwa wyświetlana w raporcie i temacie wiadomości
- `APPRISE_ENABLED`, `APPRISE_URL`, `APPRISE_URLS` — opcjonalna wysyłka przez Apprise
- `SMTP_*` — ustawienia zapasowej wysyłki SMTP

Pełny zestaw opcji znajduje się w `syncthing/.env.example`.

## Licencja

Projekt jest udostępniany na licencji **MIT License**.

## Dokumentacja

Szczegółowa dwujęzyczna instrukcja znajduje się w repozytorium jako:

```text
UGREEN_Syncthing_Reporter_Handbuch_DE-EN.pdf
```

## Wersja

- Wersja reportera: V2.2
- Obraz Docker: `ghcr.io/railsimulatornet/ugreen-nas-syncthing-reporter:latest`
- Data builda pakietu: 2026-05-21
