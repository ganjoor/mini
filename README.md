# مین‌گنجور — proof of concept

A single static file, no build step, no server — proof that `ganjoor-data` on its own (fetched
live from jsDelivr) is enough to run a small reading app for classical Persian poetry.

## What it does

- Loads `manifest.json` and shows every poet.
- Click a poet → their bio (`poet.json`) + their root category's contents (`_cat.json`).
- Click a category → its children, recursively, until you reach a poem.
- Click a poem → renders it as traditional right/left hemistich pairs (مصرع اول/دوم), with metre
  and rhyme shown when present.
- A "jump to poem by numeric id" box, demonstrating the bucketed id index
  (`index/poems-by-id/{id / 2000}.json`) resolving a bare id to a path.
- Routing is hash-based (`#/hafez/ghazal/sh1`) — works on GitHub Pages with zero server
  configuration, since there's no server-side route to configure in the first place.

Everything is fetched live, in the browser, straight from
`https://cdn.jsdelivr.net/gh/ganjoor/ganjoor-data@main/` — there is no backend anywhere in this
app. That URL is the only "API base" set at the top of the `<script>` block in `index.html`.

## Deploying it

1. Create a new, empty GitHub repo (or use an existing one).
2. Copy `index.html` into the root of that repo. That's the entire app — one file.
3. Push it.
4. Repo → **Settings → Pages** → **Source: Deploy from a branch** → pick `main` / `(root)` → **Save**.
5. GitHub gives you a URL like `https://<username>.github.io/<repo>/` within a minute or two.

No build step, no `npm install`, no config beyond that. Any static host works the same way
(Netlify, Cloudflare Pages, a plain `python -m http.server` locally to preview first) — GitHub
Pages is just the one named in the ask.

## If you want to try it locally first

```
cd mini-ganjoor
python3 -m http.server 8000
```

then open `http://localhost:8000`. (A plain `file://` open also mostly works, but some browsers
restrict `fetch()` from `file://` origins — a local server sidesteps that.)

## Known limitations (this is a proof of concept, not a full client)

- No search — `ganjoor-data` doesn't have a search index yet (documented in its own `API.md`).
- Poems with unusual structures (free verse, unusual band forms) fall back to one line per verse
  rather than fully reproducing every classical form's exact traditional layout.
- No caching/offline support, no pagination for very large categories — it's a proof of concept,
  not a production reading app.
