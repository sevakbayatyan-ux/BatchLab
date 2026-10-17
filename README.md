# BatchLab – Nesting & Production Layout

Web app for nesting print jobs (sheets, rolls, banners), Colex / SAGA marks,
cut files (PDF + Illustrator .ai with Cut/Mark layers) and double-sided imposition.

Everything runs in the browser: files you load never leave the computer.
The server only delivers the app, so any static web host works.

## What's in this folder
- `index.html`   the app
- `vendor/`      PDF, QR and ZIP libraries (no outside CDN needed)
- `Dockerfile`, `nginx.conf`   for running it on your own server

## Option A: your own server with Docker (recommended for an internal tool)
```bash
docker build -t batchlab .
docker run -d --name batchlab -p 8080:80 --restart unless-stopped batchlab
```
Then point a subdomain (e.g. `nest.yourdomain.com`) at the server and put
HTTPS in front (Caddy, nginx proxy, or Cloudflare).

Password protect it (optional):
```bash
htpasswd -c .htpasswd shop          # creates a user called "shop"
# uncomment the two auth_basic lines in nginx.conf, then add to Dockerfile:
# COPY .htpasswd /etc/nginx/.htpasswd
```

## Option B: no server to manage (Cloudflare Pages or Netlify, free)
1. Create a new Pages / Netlify site and upload this folder (drag and drop works on Netlify).
2. Add your custom domain in the site settings; HTTPS is automatic.
3. To keep it private, turn on Cloudflare Access (Zero Trust) or Netlify password protection.

## Updating
Replace `index.html` (and `vendor/` if it changed) and redeploy.
`index.html` is served with no-cache, so everyone gets the new version on the next reload.
The version tag next to the title shows which build is running.

## Notes
- Settings (defaults, cutter profiles) are saved per browser. Shared shop-wide
  defaults, job history and Google Drive export need a small backend – planned next.
