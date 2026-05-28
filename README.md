# SAP Pages Explorer

A single-file web app that auto-discovers every GitHub Pages site published on your GitHub Enterprise Server instance and gives you one place to browse, search, tag, and bookmark them all.

**Live app → [pages.github.tools.sap/I560043/github-pages-explorer](https://pages.github.tools.sap/I560043/github-pages-explorer/)**

---

## Getting started

### 1. Create a Personal Access Token

The app uses a PAT to call the GHES REST API. You only do this once.

1. Open [github.tools.sap → Settings → Tokens → New token](https://github.tools.sap/settings/tokens/new?scopes=read%3Aorg%2Crepo&description=SAP+Pages+Explorer) *(link pre-fills the required scopes)*
2. Give it a name, e.g. **SAP Pages Explorer**
3. Select at minimum:
   - `repo` — read repository metadata
   - `read:org` — list organisations
4. Click **Generate token** and copy it

> The token is stored only in your browser's `localStorage`. It is never sent anywhere except directly to `github.tools.sap`.

### 2. Sign in

Open the app, paste your token into the input field, and click **Connect**. The app will start scanning immediately.

---

## Features

### Browsing & search

| Feature | How to use |
|---|---|
| **Search** | Type in the search bar (header) — filters by repo name, description, and org in real time |
| **Filter by org** | Click an org name in the left sidebar |
| **Filter by tag** | Click a tag name in the left sidebar |
| **Starred only** | Click the ⭐ filter in the sidebar |
| **Sort** | Use the sort dropdown (Name / Org / Last updated) |

### Layouts

Switch between three view layouts using the buttons in the toolbar:

- **Table** *(default)* — compact rows, best for large result sets
- **Compact** — small cards in a dense grid
- **List** — wider cards with descriptions, one per row

### Cards & repos

- **Open site** — click a card / row to open the GitHub Pages URL in a new tab
- **Copy URL** — click the copy icon on a card to copy the Pages URL to clipboard
- **Star** — click ⭐ on a card to bookmark it; starred repos appear in the sidebar filter
- **Tags** — click the tag icon on a card to add/remove custom labels (stored locally, not in GitHub)

### Refresh & cache

Results are cached for **30 minutes** to avoid slow re-scans on every load.

- **Refresh** button (header) — clears the cache and re-scans immediately
- **Clear cache** button — removes cached data without triggering a new scan

---

## Settings

Open Settings via the ⚙ icon in the header.

### Scan mode

| Mode | Description |
|---|---|
| **My orgs only** *(default)* | Scans only orgs your token has membership in — fast |
| **All orgs on instance** | Scans every org on the GHES instance — thorough but slow on large instances |
| **Custom orgs only** | Scans only the orgs you specify in the Org filter field |

### Additional orgs

Add extra orgs that are always scanned regardless of scan mode — useful for orgs you are not a member of (e.g. `project-piper`).

- Type the org login in the input field and press **Enter** or click **Add**
- Or use **Browse** to search all orgs on the instance and pin them
- Remove an org by clicking **×** on its chip

### Theme

Choose **Light**, **Dark**, or **System** (follows your OS preference).

### Export & Import

Use these to back up your configuration or move to a new browser/machine without re-entering anything.

- **Export** — downloads a JSON file containing your token, settings, tags, starred repos, and layout preference
- **Import** — pick a previously exported file to restore everything in one click

> Keep your export file safe — it contains your PAT.

---

## Token management

| Situation | What happens |
|---|---|
| Token is valid | Stored in `localStorage`, persists across browser sessions |
| Token expires or is revoked | App detects the 401 response, clears the token, and shows the login screen with a clear message |
| New browser / machine | Use **Export** on the old browser, then **Import** on the new one |
| Sign out manually | Click **Sign out** in Settings → Authentication |

---

## How it works

1. On load, the app reads your token from `localStorage`
2. It calls `/user/orgs` (or `/organizations` for all-orgs mode) to discover orgs
3. For each org it fetches all repos, then checks each repo for a GitHub Pages site via `/repos/{owner}/{repo}/pages`
4. Pages checks run in batches of 5 in parallel to keep things fast
5. Results are cached in `localStorage` for 30 minutes

No backend, no build step, no dependencies — everything runs in your browser from a single HTML file.

---

## Development

```bash
# Clone
git clone https://github.tools.sap/I560043/github-pages-explorer.git
cd github-pages-explorer

# Serve locally (required — file:// blocks cross-origin API calls)
python -m http.server 8080

# Open
# http://localhost:8080/github-pages-explorer.html
```

The entire app lives in `github-pages-explorer.html`. Edit it, reload — no build step needed.
