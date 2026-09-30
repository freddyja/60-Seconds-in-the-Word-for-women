# Kingdom Sisters — 60 Seconds in the Word

A one-minute devotional app for Kingdom Sisters — English, Spanish, and Portuguese. Rose/gold color palette and sister-facing language. Fifteen thematic devotionals, plus a second track that walks Proverbs 31 in nine short sections. Installable on phones as a Progressive Web App (PWA): add it to the home screen and it opens full-screen, no browser bar, works offline.

## Files

```
.
├── index.html              ← the whole app (content + logic lives here)
├── manifest.json           ← tells phones how to install it as an app
├── service-worker.js       ← caches the app so it works offline
├── icons/
│   ├── icon-192.png
│   ├── icon-512.png
│   ├── icon-maskable-512.png
│   └── apple-touch-icon.png
└── README.md
```

## Put it on GitHub Pages (free hosting)

1. Create a new repo on GitHub (e.g. `kingdom-sisters-devotional`). Public repos get free Pages hosting; a private repo needs GitHub Pro/Team/Enterprise for Pages.
2. Upload all the files above, keeping the folder structure — `icons/` must stay a subfolder, not flattened.
3. In the repo: **Settings → Pages → Build and deployment → Source → Deploy from a branch**, pick `main` and `/ (root)`, then **Save**.
4. Wait a minute or two, then your app is live at:
   `https://<your-username>.github.io/<repo-name>/`

That's it — no build step, no server, nothing to compile. This is a completely separate app/repo from the Fire Fellowship one, so the two can be installed side by side on the same phone without conflicting.

## Installing it on a phone

- **Android / Chrome:** open the link above. A small "⤓ Install App" button appears near the top — tap it, or use Chrome's menu → "Add to Home screen."
- **iPhone / Safari:** open the link, tap the Share icon, then "Add to Home Screen." (iOS doesn't support the automatic install prompt — this manual step is required by Apple, not a bug.)

## Updating the devotional content

All devotionals live inside `index.html`, in the `<script>` tag. Thematic studies are the `DEVOTIONALS` array. The Proverbs 31 track is the `PROVERBS31` array. Each entry looks like:

```js
{
  id: "beloved",
  en: { ref: "...", verse: "...", title: "...", body: [...], questions: [...] },
  es: { ... },
  pt: { ... }
}
```

There are 15 thematic devotionals (Chosen, Peace, and Strength, kept as they were, plus Beloved, Worth, Speech, Friendship, Belonging, Legacy, Forgiveness, Courage, Calling, Hospitality, Waiting, and Prayer). `PROVERBS31` holds nine chapter sections: 31:1-9, 10-12, 13-15, 16-18, 19-22, 23-25, 26-27, 28-29, and 30-31.

The screen has a Themes / Proverbs 31 switch (Temas / Proverbios 31 / Provérbios 31). Previous, next, the dots, and Mark Read stay inside the track that is open. The first visit opens on today's theme (day-of-year). After that, the app remembers the last track and place in `localStorage` (`kingdom_sisters_place`). To add another theme or section, copy one whole block into the matching array. The language switcher picks it up automatically.

**Important:** every time you push a content or code change, bump the version number at the top of `service-worker.js`:

```js
const CACHE_NAME = 'kingdom-sisters-60-seconds-v7';  // bump on every content or code change
```

Without that bump, phones that already installed the app will keep showing the old cached version instead of picking up your edit.

## Testing locally before you push

You can just double-click `index.html` to preview it, but the install prompt and offline caching only work when served over `http(s)`, not opened directly as a file. If you have Python installed:

```bash
cd path/to/this/folder
python3 -m http.server 8000
```

Then visit `http://localhost:8000` in your browser.

## Optional: a real app-store app later

This PWA already behaves like a real app on the home screen. If you eventually want it in the Apple App Store / Google Play specifically, this same code can be wrapped with a tool like Capacitor or PWABuilder with no rewrite needed — that's a later step, not something you need now.
