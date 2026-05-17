# BAND-MAID Playlists

A standalone web app for building, playing, and sharing custom BAND-MAID YouTube playlists. Pulls the master video list from the [BAND-MAID_gpt](https://drivetimebm.github.io/BAND-MAID_gpt/youtube/youtube.json) data feed and lets fans submit their own curated playlists to a community list.

**Live site:** https://drivetimebm.github.io/BAND-MAID_playlists/

## Features

- Drag-and-drop playlist builder with all official BAND-MAID YouTube content grouped by type
- Embedded YouTube player with auto-advance, loop, and shuffle
- Local save/load of personal playlists (stored in browser)
- Community playlist library loaded from this repo
- Built-in submission flow — either via GitHub issue or copy-to-clipboard

## Repo structure

```
/
├── index.html                    # The app
├── playlists.json                # Manifest of all community playlists
├── playlists/                    # Individual playlist files
│   └── <slug>.json
├── Add-Playlist.ps1              # Curator helper to add a submission
└── .github/ISSUE_TEMPLATE/
    └── playlist-submission.yml   # Submission form
```

## Playlist file schema

Each file in `/playlists/` is a self-contained playlist:

```json
{
  "name": "My Awesome Playlist",
  "createdBy": "FanHandle",
  "description": "Short description of the theme or mood.",
  "videoIds": ["abc123", "def456", "..."]
}
```

## Index file schema (`playlists.json`)

A flat array of metadata so the app can render the dropdown without fetching every playlist file:

```json
[
  {
    "name": "My Awesome Playlist",
    "createdBy": "FanHandle",
    "description": "Short description of the theme or mood.",
    "file": "my-awesome-playlist.json",
    "count": 12
  }
]
```

## Submitting a playlist

1. Build your playlist in the app.
2. Click **📤 Share / Submit**.
3. Fill in the name, your handle, and a description.
4. Either:
   - **Submit via GitHub** (recommended) — opens a pre-filled issue. Just review and submit.
   - **Copy JSON** — copies the JSON text so you can DM it on Discord or paste it elsewhere.

## Curating submissions (maintainer notes)

When a submission comes in via GitHub Issue:

```powershell
# Copy the JSON block from the issue, then:
.\Add-Playlist.ps1 -FromClipboard

# Or save the JSON to a file first:
.\Add-Playlist.ps1 -JsonPath .\submission.json
```

The script will:
- Write the playlist to `/playlists/<slug>.json`
- Update `playlists.json` with the new entry (sorted alphabetically)
- Confirm the count and creator

Then commit and push. The site updates automatically.

## Configuration

If forking, update these constants near the top of the script block in `index.html`:

```js
const GITHUB_OWNER = 'DriveTimeBM';
const GITHUB_REPO  = 'BAND-MAID_playlists';
```

## Credits

Data source: [BAND-MAID_gpt](https://github.com/DriveTimeBM/BAND-MAID_gpt). Part of the unofficial [BAND-MAID Unofficial](https://drivetimebm.github.io/BAND-MAID_unofficial/) fan app suite.
