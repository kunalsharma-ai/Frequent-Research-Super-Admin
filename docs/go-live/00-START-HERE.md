# Frequent Research website, build 82.3: go-live pack (start here)

**Build 82.3 (12–13 November 2026) is the version to launch.** It is build 82.2 (new Meet our Team photos, phone preview fix) plus the onboarding links, the redesigned numbers and certifications, and the language button back in the header. It is a React + Vite frontend and an Express + MongoDB
backend in two Node 24 containers, with the super admin panel at `/admin`. Everything you need is in this folder and
next to it in `build82.3/`. What the recent builds added:

- **Build 82.3: onboarding links, numbers, certifications, languages, alt text.** Links to the new client and new
  vendor onboarding forms on home, Let's Talk, Scan & Connect and the footer (addresses editable in super admin →
  Onboarding → "Onboarding links on the site"). Numbers band with calm navy cards and line icons. Certification
  badges orbit and float. The language button (130+ languages, sounds and effects) is back in the header. Leadership
  messages keep the build 82.2 card design. Every image has an alt attribute; the full list is in
  `frequent-research-alt-text-build82.3.csv`.

- **Build 82: client and vendor onboarding in the super admin.** Two new spaces (Onboarding → Client onboarding,
  Vendor onboarding) show every detail of every submission, editable, with MSAs, documents, renewals, notes and
  history, and let you change the form fields of both portals. How to connect: `16-ONBOARDING-PORTALS.md`.
- **Build 82: security.** Tax and bank numbers and onboarding documents are encrypted in the database, the database
  is on a private network with its own limited user, and the web server and API add security headers and request
  limits. What the server team must do at go-live: `17-SECURITY.md`.
- **Build 81.2: logo and panels.** The original logo with the white inside the letters made transparent; the
  Caregiver and Patient panel tabs show only MyHealthOpinions.
- **Build 81: a new home hero.** Each home slide has its own moving picture: a research lab (Market Research), a
  campaign going live on a phone (Digital Marketing), code becoming an app (IT & Development), a ledger desk with a
  GST stamp (Accounts & Finance) and a 3D universe where the 7 Sampledge products orbit as planets. Slides change every
  4.5 seconds; the lag (live 3D globe, blurred moving background) is gone.
- **Build 80.2: every page opens at once.** Every page except the Sampledge product sites shows a ready-made copy of
  its first screen straight away (like home); PageSpeed on inner pages rose from about 40 to 75–94 on phones and
  from about 65 to 97–100 on desktop (`11-PAGESPEED.md`).
- **Build 80.1: smoother pages.** Service, platform and product pages no longer lag (an old moving background is
  gone; the Sampledge 3D pictures start moving on the first touch or mouse move). SampBidx and SampCrew layout fixes.
- **Build 80: new story heroes.** All 67 service pages have a brand-new animated picture that tells that service's
  story in 12 seconds (a call being answered for CATI, a payslip printed for Payroll, a search result climbing for SEO …).
  The four platforms, the Sampledge products and All services have new moving pictures on the home slides and their
  own pages. Phones get the same animations once the page has loaded. Everything else is unchanged.
- **Build 79: new animated heroes.** Home (live 3D globe, headline that builds line by line, the four platforms
  orbiting; the 10-second festival banner on top), the 4 platform pages (each its own personality and 3D scene), All
  services and all 67 service pages (each its own colours, grading and animated picture). Sampledge product heroes,
  the rest of every page and the admin are unchanged.
- **Build 78.5: every API in the admin.** System › API shows live status and all 99 endpoints, with Try it and
  Copy as curl (`15-API-REFERENCE.md`).

- **Build 78.4: an attractive admin, and keys typed in the panel.** A new animated dashboard (greeting hero with a 3D
  orbit, live numbers, traffic and leads charts, waiting approvals, live activity), its own colours and animated
  banner for every area, 10 colour palettes each admin can choose, admin photos, Import from a file in every section
  (Excel, CSV, JSON, Word, pictures, documents), a new My account page, and the
  connection keys (email, AI, social media, SampCrew) typed in **Connections & keys** and saved encrypted in the
  database (`12-MARKETING-AND-KEYS.md`). New `14-DEVELOPER-GUIDE.md`: where every part of the code is. The public
  website is unchanged.

- **Build 78.3: who may do what in the admin.** Personal admin accounts (invitation only, password + authenticator
  app, recovery codes), eight roles plus access to particular items or pages, approvals (draft → review → publish),
  lead assignment, SEO per page and redirects, media alt text, backups, and a new look for the panel (animated logo,
  light / dark theme, sounds). There is no default login any more: make the first Super Admin on the server
  (`13-ACCESS-AND-ROLES.md`). The public website is unchanged.

- **Builds 74 to 76: PageSpeed.** 100 in accessibility, best practices, SEO and the AI check on desktop and mobile;
  performance 100 on desktop and 99 to 100 on mobile (`11-PAGESPEED.md`). The home page opens with a ready-made still
  copy of its first screen; the full site, with every animation, starts on the first touch or mouse movement. Phones
  get a still hero and no intro splash.
- **Build 77: the Google tag (GTM-TDDKS62)** first in `<head>` on every page.
- Earlier: new Sampledge products (68), per-page videos and the super admin "Page videos" (70 to 71), smoother pages
  and working PDF downloads (72), Manish Arora's new photo (73). Every change is listed in `CHANGELOG.md`.

## What you get

| File | What it is | Who |
|---|---|---|
| `frequent-research-source-build82.3.zip` | The project: `frontend/` (React + Vite), `backend/` (Express), a Dockerfile in each, `docker-compose.yml`. Build the containers from this. | Developers / IT |
| `frequent-research-deploy-build82.3.zip` | The finished website files (`www/`), for hosting without Docker. | Hosting / IT |
| `frequent-research-preview-build82.3.html` | One-file preview for review only. Never upload it, and do not judge speed from it. | Kunal |
| `frequent-research-go-live-docs-build82.3.zip` | This folder of documents. | Everyone |

## Read in this order

| # | Document | For | Covers |
|---|---|---|---|
| 1 | `01-GO-LIVE-STEPS.md` | Hosting / IT | The two containers, settings, HTTPS, hosting without Docker, rollback |
| 2 | `02-BACKEND-API.md` | Backend developer | `/api/v1` endpoints, MongoDB collections, moving build 66 data, back-ups |
| 3 | `03-PRE-LAUNCH-TESTS.md` | Kunal / QA | Tick-list to run on the live domain (desktop and phone, light and dark) |
| 4 | `04-CONTENT-SIGNOFF.md` | Kunal | Facts and claims only you can confirm before announcing |
| 5 | `05-SEO-AND-AFTER-LAUNCH.md` | Marketing | Search Console, sitemap, redirects, first-week checks |
| 6 | `06-CELEBRATIONS-ADMIN.md` | Super admin team | Home-page celebration banner 2026–2030 and the prompt for your admin panel |
| 7 | `07-GALLERY-ADMIN.md` | Super admin team | Gallery sections (Festivals, Awards, Birthdays) filled with photo and video links |
| 8 | `08-QA-REPORT.md` | Everyone | What was tested before handover and the results |
| 9 | `09-FORMS.md` | Everyone | Every form on the site, where it appears and where it sends its data |
| 10 | `10-SUPER-ADMIN.md` | Kunal / admin team | The super admin panel at `/admin`: every section and what it controls |
| 11 | `11-PAGESPEED.md` | Kunal / IT | PageSpeed scores, how the home page starts, the Google tag, server settings that matter |
| 12 | `12-MARKETING-AND-KEYS.md` | Kunal / IT | The keys for email, newsletters, AI, social media and SampCrew, where to get each, and how to type them in the panel |
| 13 | `13-ACCESS-AND-ROLES.md` | Kunal / IT | First Super Admin, inviting the team, roles and page-by-page access, approvals, recovery, backups |
| 14 | `14-DEVELOPER-GUIDE.md` | Developers | Where every part of the code is, how to run, build and release |
| 15 | `15-API-REFERENCE.md` | Developers | Every API endpoint: address, what it does, body, the right it needs |
| – | `QUALITY-CLAIMS-CHECKLIST.md` | Kunal | Quality-process claims to confirm |
| – | `README.md` (in the source zip) | Developers | Project layout, local development, how the build works |

## Launch-day checklist (short version)

1. Back up the current site and any reviews or leads you already collect.
2. Unzip the source on the server, `cp .env.example .env`, set `REVIEW_SECRET`, `KEYS_SECRET`, `PUBLIC_URL`,
   `MONGO_PASSWORD` and (optional) `LEAD_WEBHOOK`. After the first start make the first Super Admin
   (`docker compose exec backend npm run admin -- create-super-admin --email … --name …`, see `13-ACCESS-AND-ROLES.md`). For email, AI, social media and SampCrew type the keys in the panel's Connections & keys,
   see `12-MARKETING-AND-KEYS.md` (they can also be added later). Coming from build 66 with saved leads or reviews: run the import
   in `02-BACKEND-API.md` once.
3. `docker compose up -d --build` (about 25 to 30 minutes the first time), then put HTTPS in front of port 8080.
4. Send one test enquiry and one test download; check they appear in `/admin` › Leads (or your CRM).
5. Run `03-PRE-LAUNCH-TESTS.md` on desktop and phone.
6. Submit `/sitemap.xml` in Google Search Console.
7. Run PageSpeed (https://pagespeed.web.dev/) on the live home page three times, desktop and mobile, and check the
   Google tag fires in Tag Assistant (`11-PAGESPEED.md`).
