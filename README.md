# Global Biolab Atlas

Interactive world map of community biotech, DIYbio, and open science labs.

**Sources:**
- HTGAA global nodes (htgaa.org)
- DIYbio.org local labs list
- Biopunk Lab global map
- Web research for labs founded 2023+

**Current dataset:** 124 labs across 7 regions and ~45 countries.

## Netlify deploy

The site is `index.html` plus `netlify.toml`. There is no build step and no env vars.

1. In Netlify, connect this repo (Project configuration → Build & deploy → Link repository). Leave the build command empty and keep the publish directory as `.`.
2. Turn on **Forms → Enable form detection**, then redeploy. Without detection, the submit form accepts nothing.
3. Every push to `main` then deploys. A drag-and-drop of the repo folder also works.

## Submissions

The **Submit** tab is a Netlify form named `lab-submission`. Each submission goes to the Netlify dashboard under **Forms → lab-submission**. Fields: name, city, country, url, desc, founded, region, capabilities, email (optional), plus the lat/lng from the geocoder. A hidden honeypot field (`bot-field`) and Akismet filter spam.

The submitter also sees their lab on their own map at once (kept in `localStorage`). Other visitors see it only after you add it to `LABS`.

To get an email for each submission, set it in Netlify: **Forms → Form notifications → Add notification → Email notification**.

To publish a submission, copy it into the `LABS` array in `index.html` and push.

## Editing the lab dataset

The lab list lives inside the HTML file as a JSON array assigned to `const LABS`. To edit:

1. Find `const LABS = [` in the file
2. Edit/add/remove entries
3. Each entry needs: `name`, `city`, `country`, `region`, `lat`, `lng`, `sources` (array)
4. Optional: `url`, `desc`, `founded`, `status` (e.g. `"dormant"`)

Optional: `capabilities` (array): `tissue-culture`, `bsl2`

Valid `sources` values: `HTGAA`, `DIYbio`, `Biopunk`, `Recent`, `Community`
Valid `region` values: `USA-EAST`, `USA-WEST`, `Americas`, `Europe`, `Asia`, `Africa`, `Oceania`

## Geocoding new entries

The submission form uses OpenStreetMap's Nominatim service to convert city/country into lat/lng. Their usage policy asks for ≤1 request/second and a UA identifying the app. For high volume, host your own Nominatim or swap in another geocoder.

## Aesthetic

Biopunk dark theme: Space Mono + Syne, acid green (#00ff41) + orange (#ff6b00), scanline overlay, inverted dark map tiles. Edit `:root` CSS variables at the top of the file to retheme.
