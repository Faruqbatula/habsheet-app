# habsheet-app — the website, built

**Nothing is written by hand in this repository.** Every file here except this
one, `CNAME`, `.nojekyll` and `404.html` is the output of

    cd ../habsheet && npm run publish:web

which builds the web version (`expo export --platform web`, the same build the
checks use) and copies it in. Edit the app, not this.

## What it is

Habsheet in a browser, at **app.habsheet.com**, on GitHub Pages. It runs the
app itself — the same screens, job store, export and backup engine as the
iPad — with a browser underneath instead of a tablet. It is deliberately
separate from `habsheet-site` (habsheet.com) so that the people who have this
link never land on the pricing pages, and so a bad deploy here cannot touch
the marketing site.

## The three files that are not build output

- **`CNAME`** — `app.habsheet.com`. GitHub Pages reads this to serve the custom
  domain. Deleting it serves the site at `faruqbatula.github.io/habsheet-app`
  instead, which is not where anyone has been sent.
- **`.nojekyll`** — load-bearing. Pages runs Jekyll by default and Jekyll
  **skips every directory beginning with an underscore**. The entire JavaScript
  bundle is in `_expo/`, so without this file the page loads and does nothing.
- **`404.html`** — a copy of `index.html`. The site is one page; the local
  server (`npm run web`) serves the app for any unknown path and Pages cannot,
  so this makes a refresh on a stray path open the app rather than a 404.

## What is in the bundle, and what is not

The build inlines the development project's address and its **anon** key — the
key designed to be public. Isolation between workshops is held by the access
rules on the server, proven through real sign-ins by
`check-access-rules-hosted`, not by hiding the key. **No service-role key and
no Stripe key is in the bundle**; that was checked against this build before it
was first published.

Nothing of a customer's work is reachable without signing in: measured in
Chromium against this exact build, a visitor with no session gets a sign-in box,
no sheet, and no off-origin request made before signing in.

## Nothing is kept in the browser

The tab holds the workshop in memory while it is open and keeps no copy
(`src/web/webFs.ts`). Close the tab and anything the server refused is gone.
