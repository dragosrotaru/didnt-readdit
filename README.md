# Didn't Readdit

A Chrome extension that adds a "TLDR" button to link posts on Reddit, meant to show a short summary of the linked article without leaving the page. Built with Adrian Fekete in 2016.

> Archived: no longer maintained. Kept for reference.

## What it does

- A background script watches for Reddit tabs and injects a content script, stylesheet and jQuery once the page has loaded.
- The content script finds each external link post (skipping self posts and posts that already have Reddit's own expando button) and adds a TLDR toggle next to the title.
- Summaries are cached per URL in IndexedDB. On a cache miss the extension asks the backend API for a summary, then stores it. The cache clears itself once a week.
- Clicking TLDR expands an inline panel with the summary and footer links (version, donate, feedback).

## Status

The client side works end to end, but the summarization step was never built. The Node API (`API/api.js`) returns a placeholder string ("this is the summary for <url>") so the extension could be tested against a live endpoint. Redis caching and a crawler queue are sketched in comments only. An early Django project (April 2016) was scaffolded and later removed; it never got past the default settings.

The extension targets the old Reddit layout (`.thing.link`, `.expando`) and uses Manifest V2 with a synchronous XHR, so it will not load or work on current Chrome or Reddit.

## Tech stack

- Chrome extension (Manifest V2), JavaScript, jQuery 2.2, IndexedDB
- Node.js, Express, Helmet (API), deployed to Heroku via `Procfile`
- Redis (planned, not wired up)

## Layout

```
Extensions/Chrome/   manifest, background script, content script, CSS, icons
API/                 Express server and config
Procfile             Heroku entry point (node API/api.js)
package.json         API dependencies
```

## Running it

For reference only:

```
npm install
node API/api.js          # listens on $PORT or 8080
```

Then load `Extensions/Chrome` as an unpacked extension and point `apiURL` in `didntreaddit.js` (and the host permission in `manifest.json`) at the local server.

## Context

A side project from 2016 to learn browser extension development and a small Node backend, with longer-term plans (OAuth, AWS deployment, Redis, a landing page and a Chrome Web Store release) that were not carried out. The last commits, in 2017–2018, were cleanup.
