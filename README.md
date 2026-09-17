# Kingdom Sisters — 60 Seconds in the Word

A one-minute daily devotional app for Kingdom Sisters — English, Spanish, and Portuguese. Same script as the Fire Fellowship app, rebranded: rose/gold color palette, "sister"-facing language, and its own starter set of devotionals (Chosen, Peace, Strength). Installable on phones as a Progressive Web App (PWA): add it to the home screen and it opens full-screen, no browser bar, works offline.

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

All devotionals live inside `index.html` in a JavaScript array called `DEVOTIONALS`, near the top of the `<script>` tag. Each entry looks like:

```js
{
  id: "chosen",
  en: { ref: "...", verse: "...", title: "...", body: [...], questions: [...] },
  es: { ... },
  pt: { ... }
}
```

There are 3 to start (Chosen, Peace, Strength) — the same starting shape as the Fire Fellowship app had before it grew. To add more, copy one whole `{ id: ..., en: {...}, es: {...}, pt: {...} }` block, paste it before the closing `];`, and fill in the new content. No other file needs to change — the day-of-year rotation and the language switcher pick it up automatically. Just ask Claude for more entries in the same conversation any time — it can keep writing them in this exact format and pattern them in for you.

**Important:** every time you push a content or code change, bump the version number at the top of `service-worker.js`:

```js
const CACHE_NAME = 'kingdom-sisters-60-seconds-v2';  // was v1
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
