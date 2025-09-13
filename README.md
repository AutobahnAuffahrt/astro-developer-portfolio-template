
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

## Deployment via GitHub Actions (FTPS zu Webspace)

Dieses Repository enthält einen GitHub Actions Workflow unter `.github/workflows/deploy.yml`, der bei jedem Push auf `main` ausführt:

- Node.js 18 verwenden, `npm install` ausführen
- `npm run build` → erzeugt `./dist`
- Upload der statischen Dateien via FTPS auf deinen Webspace (z. B. `www567.your-server.de`) mit `SamKirkland/FTP-Deploy-Action`

Benötigte GitHub‑Secrets (Repository → Settings → Secrets → Actions):

- `FTP_HOST` – z. B. `www567.your-server.de`
- `FTP_USER` – dein FTP‑Benutzername
- `FTP_PASSWORD` – dein FTP‑Passwort
- `FTP_REMOTE_DIR` – Zielverzeichnis auf dem Server (z. B. `/` oder `/htdocs` – je nach Hoster‑Setup)
- Optional: `FTP_PORT` – Standard ist 21 für FTPS (Explizit). Lass es leer, wenn du den Standard verwendest.

Hinweise:
- Die Action nutzt FTPS (FTP über TLS). Stelle sicher, dass dein Webspace FTPS unterstützt (bei klassischen Hosting‑Paketen üblich).
- `server-dir` ist relativ zur FTP‑Root deines Accounts. Viele Hoster verwenden ein Web‑Root wie `/htdocs` oder `/public_html`. Prüfe das in deinem Hosting‑Panel oder Client.
- Der Editor kann evtl. eine Warnung „Unable to resolve action“ für `SamKirkland/FTP-Deploy-Action` zeigen. In GitHub Actions läuft die Action unter `@v4` regulär.

Sicherheit
- Bitte keine Zugangsdaten im Repo speichern. Lege sie ausschließlich als GitHub‑Secrets an.
- Falls Zugangsdaten bereits im Repo standen (siehe frühere `ftp.md`), ändere das Passwort im Hosting‑Panel und committe keine Klartext‑Daten mehr.

Lokal testen (optional):
- Verbinde dich mit einem FTP‑Client (z. B. FileZilla) via FTPS mit `www567.your-server.de` und prüfe, welches Zielverzeichnis dem Web‑Root entspricht. Setze `FTP_REMOTE_DIR` entsprechend.
```
