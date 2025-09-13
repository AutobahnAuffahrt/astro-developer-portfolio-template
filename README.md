
# astro-developer-portfolio-template

Mein persönliches Entwickler-Portfolio, basierend auf dem [Astro Developer Portfolio Template](https://github.com/devidevio/astro-developer-portfolio) von devidev.io.

**Eigene Anpassungen:**

- Bio und Tech Stack direkt aus Markdown (`bio.md`)
- Projekte, Social Links und Layout individuell angepasst
- Modernes Card- und Markdown-Styling

Das ursprüngliche Template stammt von [devidev.io](https://devidev.io) und ist unter MIT-Lizenz verfügbar.

Achtung: Dieses Repository ist ein Fork mit eigenen Anpassungen und dient als persönliche Portfolio-Seite.

## Demo

Live: [schuchardt.cloud](https://schuchardt.cloud)
GitHub-Profil: [AutobahnAuffahrt](https://github.com/AutobahnAuffahrt)

## Features

- No bundled JavaScript – optimized for performance and speed.
- Fully responsive – mobile-friendly and adaptable across all devices.
- SEO & Social Media Ready – includes OpenGraph, Twitter, and DublinCore metadata.
- 100/100 Google PageSpeed Score – for both mobile and desktop.
- Code highlighting – clean and readable syntax with [Shiki](https://github.com/shikijs/shiki).
- Developer Portfolio & Projects Showcase – display your work with ease.
- Code Editor-Inspired Design – modern and developer-friendly aesthetics.

## Tech Stack

- [Astro](https://astro.build/)
- [TailwindCSS](https://tailwindcss.com/)
- [Shiki](https://github.com/shikijs/shiki)

## Getting Started

```sh

# 1. Repository klonen
git clone https://github.com/AutobahnAuffahrt/astro-developer-portfolio-template .

# 2. Install dependencies
npm install

# 3. Run the development server
npm run dev

# 4. Build for production
npm run build

# Deploy the contents of the `./dist` folder wherever you like.

## Deployment via GitHub Actions (Hetzner)

Dieses Repository enthält einen GitHub Actions Workflow unter `.github/workflows/deploy.yml`, der bei jedem Push auf `main` ausgeführt wird. Kurz: der Workflow verwendet Node.js 18, führt `npm install` und `npm run build` aus und deployed per rsync/SSH in ein neues Release‑Verzeichnis. Schließlich wird ein `current`‑Symlink auf das neue Release gesetzt und alte Releases werden bereinigt (standardmäßig werden 5 Behalteversionen verwendet).

Wichtige Details (aktuell im Workflow):
- Node.js: 18
- Install: `npm install` (der Workflow verwendet keinen lockfile‑Cache)
- Build: `npm run build` → Erzeugt `./dist`
- Deploy: rsync über SSH in `RE MOTE_PATH/releases/<timestamp>-<runid>`, dann `ln -sfn` auf `current`

Benötigte GitHub‑Secrets (Repository → Settings → Secrets → Actions):

- `HETZNER_SSH_KEY` – privater SSH‑Key (PEM/OPENSSH). Wichtig: der Workflow setzt den Key mit `webfactory/ssh-agent`; wenn der Key eine Passphrase hat, musst du zusätzlich das Secret `HETZNER_SSH_PASSPHRASE` anlegen und den Workflow erweitern. Einfacher: benutze einen Key ohne Passphrase für CI.
- `HETZNER_HOST` – Hostname oder IP (z. B. `www567.your-server.de`).
- `HETZNER_USER` – SSH‑Benutzer (z. B. `schuch_0` oder ein spezieller `deploy`‑User).
- `HETZNER_REMOTE_PATH` – Basiszielpfad auf dem Server (z. B. `/var/www/site`). Der Workflow legt dort `releases/` und `current` an.
- `HETZNER_PORT` – Optional: SSH‑Port (Standard: `22`). Wenn leer, verwendet der Workflow 22.

Optional (nicht als Secret):
- `KEEP_RELEASES` (im Workflow auf `5` gesetzt) – Anzahl der lokalen Releases, die behalten werden.

SSH‑Key Hinweise
- Für GitHub Actions ist es am unkompliziertesten, einen privaten Key ohne Passphrase zu verwenden und diesen als `HETZNER_SSH_KEY` zu speichern.
- Wenn du eine Passphrase verwenden willst, lege ein Secret `HETZNER_SSH_PASSPHRASE` an und erweitere den Workflow (Action `webfactory/ssh-agent` unterstützt `ssh-passphrase`).

Key erzeugen (PowerShell):

```powershell
ssh-keygen -t ed25519 -C "deploy@schuchardt.cloud" -f .\deploy_key
# Bei Passphrase‑Abfrage ENTER drücken, wenn kein Passphrase erwünscht (empfohlen für CI)
```

Public Key in Hetzner Console eintragen
- Wenn dein Provider (Hetzner) ein Feld „Öffentliche SFTP‑Schlüssel“ hat, öffne `deploy_key.pub`, kopiere den Inhalt und füge ihn dort ein (z. B. Name `github-actions-deploy`).

Private Key als GitHub Secret speichern

```powershell
Get-Content .\deploy_key -Raw | Set-Clipboard
# Dann in GitHub: Settings → Secrets and variables → Actions → New repository secret
# Name: HETZNER_SSH_KEY
```

Testen (lokal) — rsync → remote release → symlink (PowerShell):

```powershell
# Setze Port (falls nötig)
$env:PORT=22
rsync -az --delete -e "ssh -p $env:PORT" ./dist/ user@www567.your-server.de:/var/www/site/releases/test-local/
ssh -p $env:PORT user@www567.your-server.de "ln -sfn /var/www/site/releases/test-local /var/www/site/current"
```

Wichtig: rsync benötigt Shell‑Zugang (SSH). Wenn dein Hoster nur reines FTP/SFTP ohne Shell erlaubt, funktioniert dieses rsync‑Deploy nicht. In diesem Fall verwende eine SFTP/FTP‑Action in GitHub Actions (z. B. `SamKirkland/FTP-Deploy-Action`) und lege stattdessen die FTP‑Credentials als Secrets an.

Sicherheit
- Entferne sensible Zugangsdaten aus dem Repository (z. B. `ftp.md` enthält aktuell Klartext‑Zugangsdaten). Drehe/ändere Passwörter, wenn sie öffentlich waren.
- Ein Key ohne Passphrase ist nur so sicher wie dein GitHub‑Secret. Schütze deine Repository‑Secrets.

Wenn du möchtest, passe ich die Workflow‑Datei an, um eine Passphrase‑Unterstützung zu nutzen (Secret `HETZNER_SSH_PASSPHRASE`) oder um stattdessen eine SFTP‑Action zu verwenden. Sag Bescheid, welche Variante du bevorzugst.
```
