# My Personal Website

A multi-page personal website built for the **Stardance challenge** hosted by Hack Club and NASA. It features an About Me page, a Current Projects page that pulls from my Scratch profile, and a Future Endeavors page, all wrapped in a clean glassmorphism space theme.

Live at: https://suisbetter.github.io/personal_website/

## About

This repository contains every file needed to host the site on GitHub Pages. It is a front-end only project built with plain **HTML**, **CSS** (written inline in `<style>` blocks), and **vanilla JavaScript** — no frameworks, no build tools, no server.

## Features

- **Multi-page layout** built with plain `.html` files that link to each other:
  - `index.html` — home hub with cards linking to every section
  - `about.html` — profile card with a portrait and bio
  - `current.html` — live project cards pulled from my Scratch profile
  - `future.html` — planned ideas
- **Glassmorphism card design** — frosted-glass cards (translucent `rgba` background + `backdrop-filter: blur`) floating over a fixed starfield background (`space_bg.jpg`)
- **Hover interactions** — cards lift up and brighten, buttons animate on hover
- **Home button** — a fixed frosted pill button in the top-left corner of every subpage so visitors can always navigate back
- **Live Scratch API integration** — the Current Projects page fetches real project titles and thumbnails from `api.scratch.mit.edu`

## How I Made It

### Tech stack and process

I built the site over two days using **VS Code** with some AI assistance to learn HTML and CSS basics, and I leaned heavily on **W3 Schools** for reference on tags, properties, and rules. I started from a basic HTML boilerplate and built each page from the ground up, then connected them with `<a>` tags so the whole thing feels like one site instead of four standalone pages.

### The design

The theme is a space vibe: every page pins `space_bg.jpg` as a fixed, full-cover background, and all content sits in floating transparent "glass" cards. The cards use a translucent blue background (`rgba(55, 76, 146, 0.35)`) with `backdrop-filter: blur(12px)`, which gives that frosted look where the starfield shows through. I reused the same card CSS on every page so the site stays consistent, and icons/buttons follow the same glass language.

### How the pages connect

`index.html` is the hub: three clickable cards lead to the About, Current, and Future pages. Every subpage has the fixed "← Home" button in the top-left that sends visitors back. Because all links are relative (`about.html`, `current.html`, etc.), the site works from any path — locally or on GitHub Pages.

### The tricky part: pulling projects from Scratch

The Current Projects page was the hardest feature. My Scratch profile is public, so I can query `https://api.scratch.mit.edu/users/udinshmafi/projects` and get a JSON list of my projects with titles and thumbnails. The problem is **CORS**: that API only allows requests coming from `scratch.mit.edu` itself, so a normal `fetch()` from my GitHub Pages domain gets blocked by the browser and the page wouldn't load.

To fix it I did two things:
1. **Embedded a snapshot of my projects** directly in the HTML as a JSON blob. The page renders these instantly, so it always loads even with no network.
2. **Best-effort live refresh** — the page tries to fetch fresh data through a CORS proxy, and swaps in the live list if it succeeds. If that fails, it quietly keeps the saved snapshot and shows a small note instead of breaking the page.

This way the page never "fails to load" — it shows my projects first, then updates them when it can.

## Project Structure

```
personal_website/
├── index.html       # Home page - hub for all sections
├── about.html       # About Me - portrait + bio
├── current.html     # Current Projects - cards from the Scratch API
├── future.html      # Future Endeavors - planned ideas
├── README.md        # You are here
├── space_bg.jpg     # Starfield background used on every page
└── shafi-uddin-camsprep-v3-800-1.webp  # Portrait used on the About page
```

## Running Locally

Just open `index.html` in a browser. Or, for the most faithful experience, serve it from a local web server:

```
python -m http.server 8000
```

then visit `http://localhost:8000`.

## Deploying to GitHub Pages

The repo hosts the site directly from the `main` branch:

1. Go to the GitHub repo → **Settings → Pages**
2. Under **Source**, select **Deploy from a branch**
3. Choose `main` and the root `/` folder, then save
4. The site goes live at `https://<your-username>.github.io/personal_website/`

## Updating the Project Snapshot

The Current Projects page embeds a saved copy of my Scratch projects so it works even when the live API is unreachable. To refresh that snapshot:

1. Run `curl "https://api.scratch.mit.edu/users/udinshmafi/projects?limit=20"`
2. Replace the JSON inside the `<script id="projects-data" type="application/json">` block in `current.html`
3. Rebuild/deploy (or just commit and let GitHub Pages update)

Only `id`, `title`, and `image` are needed for the cards.

## Creation History

I created this website over two days using VS Code and some AI assistance to understand HTML and CSS basics, referencing W3 Schools and other resources along the way. I started with a basic HTML boilerplate and built the site from the ground up, using `<a>` tags to link the pages together and a home button to tie it all back to the start. The design iterated from simple centered text into the themed card layout you see today, and the Scratch integration came last once I figured out the CORS workaround above.