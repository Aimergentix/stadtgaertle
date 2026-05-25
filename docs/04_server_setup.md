# 04 — Server Setup (Hetzner / OVH)

> Structural law for the deploy target. The Archon executes this once per server.
> The Aimergent reads it as an infrastructure constraint and must not suggest deviations.
> All secret names align exactly with `.github/workflows/deploy-static.yml`.

---

## Phase 4.1 — SSH key pair (Archon local machine, one-time)

```bash
ssh-keygen -t ed25519 -C "stadtgaertle-deploy" -f ~/.ssh/stadtgaertle_deploy
```

- Copy the **public key** (`~/.ssh/stadtgaertle_deploy.pub`) to the server's deploy user
  `~/.ssh/authorized_keys`.
- Add the **private key** content to GitHub → Settings → Secrets → Actions:
  `DEPLOY_SSH_PRIVATE_KEY`.
- Add the remaining four secrets:

| Secret name         | Value                                    |
|---------------------|------------------------------------------|
| `DEPLOY_SSH_HOST`   | Server IP or hostname                    |
| `DEPLOY_SSH_PORT`   | `22` (or your custom SSH port)           |
| `DEPLOY_SSH_USER`   | `deploy` (not root)                      |
| `DEPLOY_SSH_PATH`   | `/var/www/stadtgaertle/` (trailing slash)|

---

## Phase 4.2 — Deploy user on server (as root, one-time)

```bash
adduser deploy --disabled-password
mkdir -p /var/www/stadtgaertle
chown deploy:deploy /var/www/stadtgaertle
```

---

## Phase 4.3 — nginx SPA routing (CRITICAL — do not skip)

React Router uses the HTML5 history API. Without `try_files … /index.html`, any direct
URL (e.g. `/impressum`, `/datenschutz`) returns a server 404 instead of loading the React app.

Create `/etc/nginx/sites-available/stadtgaertle`:

```nginx
server {
    listen 80;
    server_name stadtgaertle.rheinfelden.de;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name stadtgaertle.rheinfelden.de;

    ssl_certificate     /etc/letsencrypt/live/stadtgaertle.rheinfelden.de/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/stadtgaertle.rheinfelden.de/privkey.pem;

    root  /var/www/stadtgaertle;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    gzip on;
    gzip_types text/html application/javascript text/css application/json image/svg+xml;
    gzip_min_length 1024;
}
```

```bash
ln -s /etc/nginx/sites-available/stadtgaertle /etc/nginx/sites-enabled/
nginx -t && nginx -s reload
```

---

## Phase 4.4 — TLS via Certbot (one-time)

```bash
apt install -y certbot python3-certbot-nginx
certbot --nginx -d stadtgaertle.rheinfelden.de
certbot renew --dry-run   # verify auto-renewal works
```

---

## Phase 4.5 — Acceptance criteria

Run after first successful GitHub Actions deploy:

- `curl -o /dev/null -s -w "%{http_code}" https://stadtgaertle.rheinfelden.de` → `200`
- `curl -o /dev/null -s -w "%{http_code}" https://stadtgaertle.rheinfelden.de/impressum` → `200`
- `curl -o /dev/null -s -w "%{http_code}" https://stadtgaertle.rheinfelden.de/nonexistent` → `200`
  (server returns index.html; React renders the 404 component — correct SPA behaviour)
