# Frequent Research website, build 66: go-live steps

Read this first. It covers what to deploy, where and in which order. Build 66 changes how the site is built and run
(React + Vite frontend, Express backend, Node 24, one container each). Visitors see exactly the same site as build 65.

| File | For | What it covers |
|---|---|---|
| `01-GO-LIVE-STEPS.md` | Whoever deploys the site | Containers, settings, HTTPS, rollback (this file) |
| `02-BACKEND-API.md` | Backend developer | Every endpoint the site calls |
| `03-PRE-LAUNCH-TESTS.md` | Kunal / QA | Page-by-page checks on desktop and phone before announcing |
| `04-CONTENT-SIGNOFF.md` | Kunal | Facts, claims and items only you can confirm |
| `05-SEO-AND-AFTER-LAUNCH.md` | Marketing | Search Console, sitemap, redirects, speed checks, first-week monitoring |
| `06-CELEBRATIONS-ADMIN.md`, `07-GALLERY-ADMIN.md` | Super admin | Banner days and gallery |
| `08-QA-REPORT.md` | Everyone | What was tested and the results |
| `09-FORMS.md` | Everyone | Every form and where it appears |
| `README-developers.md` | Developers | Source layout and how to build |

## 1. What you deploy
- `frequent-research-source-build77.zip`: the project, with `frontend/` and `backend/`, a `Dockerfile` in each and a
  `docker-compose.yml`. This is what you build the containers from.
- `frequent-research-deploy-build77.zip`: the finished website files (`www/`, the same files the frontend container
  serves) for hosting without Docker, on any static web server.
- `frequent-research-preview-build77.html`: for review only. It packs the whole site into one file that opens by
  double-click. **Never upload it and do not judge speed from it.**

## 2. The three containers
| Container | Built from | Listens on | Needs |
|---|---|---|---|
| **frontend** | `frontend/Dockerfile`: Node 24 builds the site with Vite, then nginx serves it | port 80 inside (8080 on the host with compose) | `BACKEND_URL` (default `http://backend:8787`) |
| **backend** | `backend/Dockerfile`: Node 24 Alpine running Express | port 8787 (not published; only the frontend talks to it) | `REVIEW_SECRET`, `ADMIN_EMAIL_DOMAINS`, `PUBLIC_URL`, `MONGODB_URI` |
| **mongo** | the official `mongo:8.0` image | port 27017 (not published) | `MONGO_PASSWORD`, the `fr-mongo` volume |

The frontend's nginx passes every `/api/...` request to the backend, so the browser only ever talks to one address.
The backend keeps everything (leads, careers CVs, reviews, chats, visits, uploads and every admin change) in MongoDB:
**back up the database daily** (command in `02-BACKEND-API.md`). A hosted MongoDB (Atlas) works too: set `MONGODB_URI`
and remove the `mongo` service.

## 3. Deploy, step by step (Docker)
1. **Back up** the current web root and any `reviews.json` / `leads.jsonl` you already collect (import them later with
   `npm run import-files`, see `02-BACKEND-API.md`).
2. On the server (Docker 24+ with Compose v2), unzip the source and go into the folder.
3. `cp .env.example .env`, then set:
   - `REVIEW_SECRET`: a long random string (`openssl rand -hex 32`). Required.
   - `PUBLIC_URL`: the site's address (https://www.frequentresearch.com). Admin invitation links point here.
   - `ADMIN_EMAIL_DOMAINS`: email domains allowed for admins (default `frequentresearch.com`).
   - There is no admin password in `.env` any more (build 78.3). After the first start, make the first Super Admin:
     `docker compose exec backend npm run admin -- create-super-admin --email you@frequentresearch.com --name "Your Name"`
     and open the one-time link it prints. Full steps: `13-ACCESS-AND-ROLES.md`.
   - `MONGO_PASSWORD`: a long random password for the database container. Required.
   - `PUBLISH_MODE=manual` (every review waits for approval) or `auto`.
   - `LEAD_WEBHOOK`: optional https address that also receives every lead (CRM, Zapier, email relay).
   - `TRUST_PROXY`: `1` when nothing else is in front; `2` when a load balancer or Cloudflare sits in front of nginx.
   - `WEB_PORT`: the host port for the website (default 8080).
4. `docker compose up -d --build`. The first build takes about 25 to 30 minutes, because it opens every page in
   headless Chromium to write a real HTML file per page for search engines and the content list the super admin shows
   in Pages & sections. It needs internet access to download
   npm packages, Debian packages and Chromium. `PRERENDER=0` in `.env` gives a 2-minute build without those pages
   (fine for testing, not for launch: without them Pages & sections in the super admin reads each page live instead of listing its whole content).
5. Put HTTPS in front of the frontend container (your load balancer, Cloudflare, Caddy or an nginx/Traefik proxy) for
   `www.frequentresearch.com`, and redirect `frequentresearch.com` to it.
6. Check: `curl http://localhost:8080/healthz` prints `ok`, and `curl http://localhost:8080/api/v1/reviews/summary`
   answers JSON from the backend. `docker compose ps` shows both containers as healthy.
7. Clear any CDN cache. HTML is served with `no-cache`; files in `/assets/` have content hashes in their names and are
   cached for a year, so a new build always loads fresh.
8. Run the checks in `03-PRE-LAUNCH-TESTS.md`, then submit the sitemap (`05-SEO-AND-AFTER-LAUNCH.md`).

Updating later: `docker compose up -d --build` again. The `fr-mongo` volume is kept.

## 4. Without Docker
- Website: upload the contents of `www/` from the deploy zip to any static host. Apache (Hostinger, cPanel) uses the
  included `.htaccess`; for nginx use `frontend/nginx/default.conf.template` from the source (replace
  `${BACKEND_URL}` and point `include` at `www/.nginx/redirects.conf`).
- API: on a server with Node 24 (`nvm install 24`), `cd backend && npm ci --omit=dev`, copy `.env.example` to `.env`,
  fill it in (`MONGODB_URI` pointing at your MongoDB), and run `npm start` (keep it running with systemd or pm2). Proxy `/api/` from your web server to port 8787.

## 5. Rollback
Docker: `docker compose down`, check out the previous release and `docker compose up -d --build` (the database volume is
untouched). Without Docker: re-upload the backup from step 1.

## 6. Node.js version
Node.js 24 LTS everywhere: both Dockerfiles use `node:24` images, and both `package.json` files require `>=24`
(`.nvmrc` = 24). Tested with Node 24.22.2.

## 7. Speed
Every page loads one app script (about 7.8 MB, 2.3 MB with gzip) and one stylesheet before it paints, the same as
build 65. Now that the core rewrite is done and every file is a normal ES module, a later update can split the app so
each page loads only its own code. Measure with PageSpeed Insights on the live domain (`05-SEO-AND-AFTER-LAUNCH.md`).

## 8. Super admin
Everything editable (texts, numbers, photos, team, blogs, testimonials, prices, panel figures, PDFs, gallery,
celebration calendar, WhatsApp and chat, leads, reviews, live chat) is in the panel at `/admin`: see `10-SUPER-ADMIN.md`.
