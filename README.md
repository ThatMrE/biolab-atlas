# Global Biolab Atlas

Interactive world map of community biotech, DIYbio, and open science labs.

**Sources:**
- HTGAA global nodes (htgaa.org)
- DIYbio.org local labs list
- Biopunk Lab global map
- Web research for labs founded 2023+

**Current dataset:** 124 labs across 7 regions and ~45 countries.

## Quick Netlify deploy

1. Rename `biolab-atlas.html` → `index.html`
2. Drop into Netlify (drag-and-drop on app.netlify.com works) — or push to a Git repo and connect
3. Done. No build step. No env vars. No backend required for the static map.

## Submissions

In its current form, submissions are stored **per-browser** in `localStorage`. That means each visitor only sees the labs they themselves added.

To capture real community submissions you can curate:

### Option A — Netlify Forms (recommended, zero backend)

In `biolab-atlas.html`, find the comment block at the bottom of the `<script>` tag labeled "NETLIFY DEPLOYMENT NOTE." It shows the two changes:

1. Add a hidden `<form name="lab-submission" netlify hidden>...</form>` somewhere in the HTML body
2. Replace the storage call in the submit handler with a `fetch('/', {...})` POST to that form name

Submissions show up in the Netlify dashboard under your site's "Forms" tab. Email yourself when one arrives, then manually append accepted ones to the `LABS` array.

### Option B — Supabase / Firebase / your own API

Replace `getSubmissions()` and `saveSubmissions()` with calls to whatever you prefer. Pattern is the same: read on boot, write on submit.

## Editing the lab dataset

The lab list lives inside the HTML file as a JSON array assigned to `const LABS`. To edit:

1. Find `const LABS = [` in the file
2. Edit/add/remove entries
3. Each entry needs: `name`, `city`, `country`, `region`, `lat`, `lng`, `sources` (array)
4. Optional: `url`, `desc`, `founded`, `status` (e.g. `"dormant"`)

Valid `sources` values: `HTGAA`, `DIYbio`, `Biopunk`, `Recent`, `Community`
Valid `region` values: `USA-EAST`, `USA-WEST`, `Americas`, `Europe`, `Asia`, `Africa`, `Oceania`

## Geocoding new entries

The submission form uses OpenStreetMap's Nominatim service to convert city/country into lat/lng. Their usage policy asks for ≤1 request/second and a UA identifying the app. For high volume, host your own Nominatim or swap in another geocoder.

## Aesthetic

Biopunk dark theme: Space Mono + Syne, acid green (#00ff41) + orange (#ff6b00), scanline overlay, inverted dark map tiles. Edit `:root` CSS variables at the top of the file to retheme.
