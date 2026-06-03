# CLAUDE.md — ballarat-commercial-service-centre

> Read this file at the start of every session before doing any other work.

---

## What this repo is

Static HTML landing pages for **Ballarat Commercial Service Centre (BCSC)** — a Mechanic Marketing client. Hosted on Cloudways.

**Main website:** https://ballaratcsc.com.au/  
**Repo:** `teddi-coder/ballarat-commercial-service-centre`  
**Local path:** `/Volumes/KINGSTON/ballarat-commercial-service-centre/`

---

## Pages

| Page | Path |
|---|---|
| Home | `index.html` |
| Bus Mechanical | `bus-mechanical/index.html` |
| Bus RWC | `bus-rwc/index.html` |
| Bus Safety Inspections | `bus-safety-inspections/index.html` |
| Heavy Vehicle Service (Ballarat) | `heavy-vehicle-service-ballarat/index.html` |
| Heavy Vehicle Service & Repair | `heavy-vehicle-service-repair/index.html` |
| Truck Inspections | `truck-inspections/index.html` |
| Truck Repair & Servicing Ballarat | `truck-repair-servicing-ballarat/index.html` |
| Truck RWC | `truck-rwc/index.html` |
| Privacy Policy | `privacy-policy/index.html` |
| Terms of Service | `terms-of-service/index.html` |
| Thank You | `thank-you-for-booking/index.html` |

---

## Session State — 2026-06-03

### What was done

**WhatConverts form tracking fix (merged to `main`):**
- 7 subpages had no `<form>` wrapper — contact sections used `<div class="quote-card">` with a dead `<a href="#">` submit button and no `name` attributes on inputs
- Fixed: added `<form id="booking-form" novalidate>` wrapper, `name` attrs on all fields, replaced `<a>` with `<button type="submit">`, wired `wc_capture_form()` on submit
- `index.html` and `truck-repair-servicing-ballarat` had existing `<form>` tags — added missing `name` attrs only

**Branding fix (merged to `main`):**
- Logo: was `white-logo.svg` (flat white, missing road stripe). Replaced with `BSCS_Logo_white.webp` downloaded from ballaratcsc.com.au. Applied across all 12 pages (header + footer).
- Hero: `index.html` was showing `exterior.jpg` (building photo). Replaced with `hero-truck.webp` (red truck highway shot matching main site).
- Two new files added to `/images/`: `BSCS_Logo_white.webp`, `hero-truck.webp`

### Outstanding — form does not send data

The form currently shows a "Thanks!" message on submit but **sends no data anywhere**. No email is sent, nothing is stored.

**Root cause:** A Cloudflare Worker (`ballarat-form-handler`) was built to receive POSTs and email via Resend, but it has never been deployed or wired into the site.

**Fix needed:**
1. Deploy the worker:
   ```bash
   cd /Volumes/KINGSTON/ballarat-form-handler
   wrangler deploy
   # note the *.workers.dev URL
   ```
2. In each page's submit handler JS, add a `fetch()` POST to that URL — all `name` attributes are now correctly set, so the POST will work immediately.

---

## Do NOT

- Do NOT change `action` attributes on forms without confirming — the worker endpoint will go here
- Do NOT rename input `name` attributes — the worker expects specific field names
- Do NOT change any WhatConverts script tags — tracking ID is `136716`... wait, BCSC may have a different ID. Verify with WhatConverts before touching scripts.
- Do NOT modify subpage hero images (bus-service, truck-inspect, etc.) — these are correct and page-specific

---

## WhatConverts

Tracking script is present on all pages. Tracking ID: confirm in `<head>` of `index.html` before making any changes.  
Form ID: `id="booking-form"` (set on all form wrappers as of 2026-06-03).
