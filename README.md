# Trinode Links

WhatsApp link page for **trinode.ahmad-zaven.io**. Static site served by nginx in Docker.

```
Dockerfile        nginx:alpine image, serves /site on port 80
nginx.conf        caching, gzip, SPA fallback
site/index.html   the page (numbers + pre-filled message are at the top of the <script>)
site/assets/      logos, og-image (link preview), apple icon
site/favicon.svg
```

## 1. DNS

At your DNS provider for `ahmad-zaven.io`, add:

| Type | Name      | Value                 |
|------|-----------|-----------------------|
| A    | `trinode` | your Coolify server IP |

If you use Cloudflare, set it to **DNS only** (grey cloud) until Coolify has issued the SSL certificate.

## 2. Push to Git

Coolify deploys from a Git repo, not a zip upload.

```bash
cd trinode-links-site
git init && git add . && git commit -m "Trinode links page"
git branch -M main
git remote add origin git@github.com:<you>/trinode-links.git
git push -u origin main
```

## 3. Coolify

1. **Projects → your project → + New → Resource**
2. Pick **Public Repository** (or **Private Repository (GitHub App)** if the repo is private) and paste the repo URL. Branch: `main`.
3. **Build Pack: Dockerfile** (base directory `/`).
4. **Ports Exposes: `80`**
5. **Domains:** `https://trinode.ahmad-zaven.io`
6. **Deploy.** Coolify gets the Let's Encrypt certificate automatically once the DNS record points at the server.

## Editing

Open `site/index.html`, find this near the bottom:

```js
const lines = [
  { name: "Zaven Jooharian", number: "0995650958" },
  { name: "Ahmad Sabagh", number: "0987323677" },
  { name: "Zohrab Arslanian", number: "0993181867" },
];
const message = "مرحبا Trinode 👋 حابب استفسر عن خدماتكم";
```

Change the names, numbers or the message, commit, push, and redeploy (or turn on **Auto Deploy** in Coolify so every push goes live).

Numbers are written locally (`09…`); the page converts them to `+963…` for the WhatsApp link.

## Run locally

```bash
docker build -t trinode-links . && docker run --rm -p 8080:80 trinode-links
# open http://localhost:8080
```
