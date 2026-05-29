# SAP Pages Explorer

A single-file web app that auto-discovers every GitHub Pages site published on your GitHub Enterprise Server instance and gives you one place to browse, search, filter, tag, and bookmark them all.

**Live app (SAP internal) → [pages.github.tools.sap/I560043/github-pages-explorer](https://pages.github.tools.sap/I560043/github-pages-explorer/github-pages-explorer.html)**
**Live app (public) → [skalmodiya.github.io/github-pages-explorer](https://skalmodiya.github.io/github-pages-explorer/github-pages-explorer.html)**

---

## Getting started

### 1. Sign in

When you open the app you'll see a two-step login screen:

1. Click **Open GitHub to generate token ↗** — the GitHub token creation page opens with the required scopes pre-filled
2. Scroll down on that page and click **Generate token**, then copy the token
3. Come back to the app and paste it into the input field, then click **Connect**

> Your token is stored only in your browser's `localStorage`. It is never sent anywhere except directly to `github.tools.sap`.

**Required scopes:** `repo` and `read:org`

If your token expires or is revoked, the app shows an error and takes you straight to the paste step so you can reconnect quickly.

---

## UI overview

```
┌─────────────────────────────────────────────────────────────────┐
│  Header: Logo · Search bar · Action buttons · Theme toggle      │
├──────────────┬──────────────────────────────────────────────────┤
│              │  Toolbar: Sort · Filters · View toggle           │
│   Sidebar    │─────────────────────────────────────────────────-│
│              │                                                   │
│  Orgs list   │  Main area: Cards / Table rows                   │
│  Tags        │  (streams in as results are found)               │
│  Starred     │                                                   │
└──────────────┴──────────────────────────────────────────────────┘
```

---

## Features

### Authentication

| Feature | Details |
|---|---|
| **Guided token creation** | "Open GitHub to generate token" button pre-fills required scopes — no manual scope selection |
| **Token persistence** | Stored in `localStorage`; survives browser restarts without re-entering |
| **Expiry detection** | On a 401 response the app clears the token and goes straight to the paste step with a clear message |
| **Sign out** | Settings → Authentication → **Sign out** |
| **Export / Import** | Back up your token and all settings to a JSON file; restore on a new browser or machine |

---

### Scan modes

Choose how the app discovers organisations in **Settings → Scan mode**:

| Mode | Description |
|---|---|
| **My orgs only** *(default)* | Only orgs your token has membership in — fastest |
| **All orgs on instance** | Every org on the GHES instance — thorough, slower on large instances |
| **Pick orgs from instance** | Browse the full org list and check the ones you want |
| **Custom orgs only** | Only the orgs you explicitly specify in the Org filter field |

Results stream in live — new pages appear as each org finishes scanning.

---

### Organisation sidebar

The left sidebar lists all discovered organisations:

- **Filter orgs** — type in the search box at the top to narrow the list
- **Click an org** — view only pages from that org; an org banner appears with avatar, description, member count, blog, and location
- **Count badge** — shows how many Pages sites were found per org
- Click the logo or **Show all** to return to the full results

---

### Search

The **search bar in the header** filters across all loaded results in real time:

- Repo name
- Organisation name
- Description
- Tags
- Pages URL

Debounced — results update 200 ms after you stop typing.

---

### Toolbar filters

All filters sit in the toolbar above the results. They stack — multiple filters apply together. Active filters highlight in blue. When any filter is set a **✕ Clear filters** button appears to reset everything at once.

Filters are ordered to match the table columns:

| Filter | Type | What it does |
|---|---|---|
| **Sort** | Dropdown | Name A–Z / Z–A · Recently Updated / Oldest Updated · Org A–Z / Z–A |
| **Filter name…** | Text input | Substring match on repo name (live as you type) |
| **Org** | Dropdown | Exact match; auto-populated with all orgs from loaded results, sorted A–Z |
| **Tags** | Dropdown | All · Has tags · No tags |
| **Status** | Dropdown | All · Active · Archived |
| **Starred** | Dropdown | All · Starred only |

The results count in the toolbar always reflects the current filtered set (`N of M sites`).

---

### Sorting

**Toolbar sort dropdown** — six options:

- Name A–Z / Z–A
- Recently Updated / Oldest Updated
- Org A–Z / Z–A

**Clickable table headers** (table view only) — click **Name**, **Org**, or **Updated** to sort by that column; click again to reverse direction. A ▲/▼ indicator shows the active sort and direction.

---

### Views

Switch between three layouts using the icons on the right of the toolbar:

| View | Best for |
|---|---|
| **Table** | Large result sets — compact rows with all columns visible |
| **Compact grid** | Quick visual scanning — small cards in a dense grid |
| **List** | Reading descriptions — wider cards, one per row |

Your last-used view is remembered across sessions.

---

### Table columns

| Column | Description |
|---|---|
| **Name** | Clickable link to the GitHub Pages site; sortable |
| **Org** | Org avatar + name; sortable |
| **Description** | Repository description (truncated) |
| **Tags** | All tags for this page — click a tag to filter by it |
| **Updated** | Relative time (e.g. "2 d ago"); sortable |
| **Status** | Live or Archived badge |
| **Actions** | Star · Edit tags · Copy URL · Open repo |

Starred rows are highlighted with an orange left border.

---

### Actions on each page

These appear in the **Actions** column (table) or on each card (other views):

| Action | Icon | What it does |
|---|---|---|
| **Star** | ⭐ | Bookmark the page; starred pages show in the Starred sidebar filter |
| **Edit tags** | 🏷 | Open the tag editor to add or remove custom labels |
| **Copy URL** | 📋 | Copies the Pages URL to clipboard with a brief confirmation flash |
| **Open repo** | ↗ | Opens the GitHub repository in a new tab |

Manual pages also have **Edit** and **Delete** actions.

---

### Tags

Tags are custom labels you apply to pages — stored locally, not in GitHub.

- Click the tag icon (🏷) on any card or table row to open the **tag editor**
- Type a tag name and press **Enter** or **,** to add it
- Click an existing tag chip to remove it; **Backspace** removes the last tag when the input is empty
- Tags appear as coloured badges in cards and table cells
- Click any tag badge to instantly filter by that tag
- The **sidebar** lists all tags in use with a count of how many pages have each tag
- The **Tags filter** in the toolbar lets you filter to tagged / untagged pages

---

### Starring / Favourites

- Click ⭐ on any card or table row to star/unstar it
- The **sidebar** shows a starred count and a one-click filter to see only starred pages
- The **Starred dropdown** in the toolbar also filters to starred only
- Starred state persists in `localStorage`

---

### Manual pages

Add any GitHub Pages URL that was not auto-discovered:

1. Click **Add page** in the header
2. Enter the URL, a display name, org, and optional description
3. Click **Save**

Manual pages are visually marked with a **Manual** badge and support the same actions (star, tags, copy, edit, delete) as auto-discovered pages.

You can also add pages from the **Search & Add** modal (see below).

---

### Search & Add

The **Search & Add** button in the header opens a modal that scans across all orgs for GitHub Pages sites:

- Type an org name or search term — results stream in as orgs are scanned
- Click **Add** on any result to add it to your collection as a manual page
- Useful for one-off discoveries without changing your main scan mode

---

### Settings

Open via the ⚙️ button in the header.

| Setting | Description |
|---|---|
| **GitHub Enterprise Instance** | Auto-detected from the page URL — read-only |
| **Authentication** | Shows connected status; Sign out button |
| **Org filter** | Comma-separated org logins used in "Custom orgs" scan mode |
| **Scan mode** | My orgs / All orgs / Pick orgs / Custom (see above) |
| **Pick orgs** | Searchable checkbox list when scan mode is "Pick orgs" |
| **Additional orgs** | Orgs that are always scanned regardless of scan mode — type and press Enter, or use Browse to search the instance |
| **Theme** | Light · Dark · System (follows OS preference) |
| **Auto-refresh on load** | Automatically scan when the page loads |
| **Show archived** | Include archived repositories in results |
| **Export** | Download all settings, tags, starred repos, and token as a backup JSON |
| **Import** | Restore everything from a previously exported backup file |

---

### Theme

Three modes available via **Settings** or the 🌗 button in the top-right corner of the header:

- **Light** — GitHub-style light theme
- **Dark** — GitHub-style dark theme
- **System** — automatically follows your OS preference

---

### Caching & performance

- Results are **cached per org for 30 minutes** in `localStorage` — reopening the app is instant
- **Refresh** (header button) — clears the cache and re-scans all orgs immediately
- **Clear Cache** (header button) — removes cached data without triggering a new scan
- Pages load **20 at a time** and more are appended as you scroll (infinite scroll)
- Org scans run with up to **10 concurrent repo checks** so large orgs finish quickly

---

### Export & Import

Use these to back up your data or move to a different browser or machine:

1. In **Settings**, click **Export** — saves a `sap-pages-explorer-backup-<date>.json` file
2. On the new browser, open Settings → click **Import** → pick the file

The backup includes: token, scan settings, tags, starred repos, view preference, manual pages, and the org cache.

> Keep your export file safe — it contains your Personal Access Token.

---

## Keyboard shortcuts

| Shortcut | Location | Action |
|---|---|---|
| `Enter` | Login — token input | Submit token |
| `Enter` | Settings — Additional orgs input | Add org |
| `Enter` or `,` | Tag editor | Add tag |
| `Backspace` | Tag editor (empty input) | Remove last tag |
| `Enter` | Org browser — search input | Search orgs |

---

## How it works

1. On load, the app reads your token from `localStorage`
2. Calls `/user/orgs` (or `/organizations` for all-orgs mode) to list orgs
3. For each org, fetches all repos, then checks each for a Pages site via `/repos/{owner}/{repo}/pages`
4. Checks run in parallel batches of 5 per org; up to 10 orgs scanned concurrently
5. Results are cached in `localStorage` for 30 minutes; the UI updates in real time as pages are found

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

The entire app lives in `github-pages-explorer.html`. Edit it and reload — no build step needed.
