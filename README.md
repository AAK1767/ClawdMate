# ClawdMate

A browser extension for **Chrome** and **Firefox** that adds bulk downloading of course materials from [PESU Academy](https://www.pesuacademy.com/Academy/). Download slides, notes, assignments, question banks, and question answers — merge PDFs, zip files, or download individually — right from the course page.

## Features

- **ClawdMate tab** — appears alongside your course unit tabs
- **Multiple resource types** — toggle between Slides, Notes, Assignments, QB, and QA
- **PDF merge** — all PDF files combined into a single file via pdf-lib
- **ZIP bundling** — non-PDF files (PPTX, DOC, etc.) bundled into one ZIP via JSZip
- **Individual downloads** — click any file to download it separately
- **Caching** — switching between unit tabs and resource types is instant after the first fetch
- **Auto-detect** — panel updates automatically when you switch unit tabs
- **Progress tracking** — visual progress bar while scanning and downloading
- **Smart naming** — ZIP entries named `(number)_title.ext` with duplicate disambiguation

## Installation

### Download (easiest)

1. Go to the [Releases page](https://github.com/AAK1767/ClawdMate/releases/latest)
2. Download the zip for your browser:
   - **Chrome** → `ClawdMate-Chrome-vX.X.X.zip`
   - **Firefox** → `ClawdMate-Firefox-vX.X.X.zip`
3. Unzip it to a folder
4. Load the extension:
   - **Chrome**: Open `chrome://extensions` → enable **Developer mode** → **Load unpacked** → select the unzipped folder
   - **Firefox**: Open `about:debugging` → **This Firefox** → **Load Temporary Add-on** → select `manifest.json` inside the unzipped folder

### From source (developer mode)

1. Clone and build:
   ```bash
   git clone https://github.com/AAK1767/ClawdMate.git
   cd ClawdMate
   ./build.sh          # or .\build.ps1 on Windows
   ```

2. **Chrome**:
   - Open `chrome://extensions` → enable **Developer mode**
   - Click **Load unpacked** → select the `build/chrome` folder

3. **Firefox**:
   - Open `about:debugging` → **This Firefox** → **Load Temporary Add-on**
   - Select `build/firefox/manifest.json`

### Prerequisites

- Google Chrome (or Chromium-based browser) **or** Mozilla Firefox 91+
- A valid PESU Academy student account

## Usage

1. Log in to [PESU Academy](https://www.pesuacademy.com/Academy/)
2. Navigate to **My Courses → [Your Subject] → Course Units**
3. Click the **ClawdMate** tab that appears at the end of the unit tabs
4. Select a resource type: **Slides**, **Notes**, **Assignments**, **QB**, or **QA**
5. Click **Merge & Download** to get everything, or click individual files

> **Note:** Please disable any download managers (IDM, FDM, JDownloader, etc.) or their browser extensions before using ClawdMate. They intercept file downloads and interfere with PDF merging and ZIP bundling.

## How it works

The extension injects a content script into PESU Academy pages. It uses the academy's internal APIs (the same ones the website uses) to discover all downloadable files for the active unit, then fetches and processes them client-side.

| Step | What happens |
|------|-------------|
| 1 | Extracts the subject ID from the page DOM |
| 2 | Fetches all units and matches the active tab |
| 3 | Fetches classes for the active unit |
| 4 | Scans each class page for download links (per resource type) |
| 5 | Merges PDFs / zips other files client-side |

All processing happens in your browser. No data is sent to any third-party server.

## Supported resource types

| Type | ID | Description |
|------|----|-------------|
| Slides | 2 | Lecture slide decks (PDF/PPTX) |
| Notes | 3 | Course notes and handouts |
| Assignments | 5 | Assignment documents |
| QB | 6 | Question bank files |
| QA | 7 | Question & answer documents |

## Project structure

```
ClawdMate/
├── src/                         # Shared source (both browsers)
│   ├── content.js               # Main content script — UI + fetch + merge logic
│   ├── popup.html               # Extension popup (toolbar icon click)
│   ├── panel.css                # Styles for the download panel
│   ├── lib/
│   │   ├── pdf-lib.min.js       # PDF merging library (v1.17.1)
│   │   └── jszip.min.js         # ZIP creation library (v3.10.1)
│   └── icons/
│       ├── icon16.png
│       ├── icon48.png
│       └── icon128.png
├── platforms/
│   ├── chrome/
│   │   └── manifest.json        # Chrome Manifest V3
│   └── firefox/
│       ├── manifest.json        # Firefox Manifest V2
│       └── loader.js            # MAIN-world script injector
├── build.ps1                    # Build script (Windows)
├── build.sh                     # Build script (Linux/macOS)
├── build/                       # Assembled extensions (gitignored)
│   ├── chrome/
│   └── firefox/
├── LICENSE                      # MIT License
└── docs/
    ├── CHANGELOG.md             # Release history
    ├── CODE_OF_CONDUCT.md       # Community standards
    ├── CONTRIBUTING.md          # Contribution guidelines
    ├── DEVELOPER.md             # Technical architecture docs
    └── SECURITY.md              # Security policy
```

## Tech stack

- **Chrome**: Manifest V3 — content scripts injected with `"world": "MAIN"`
- **Firefox**: Manifest V2 — content script injects page-level `<script>` tags via `loader.js`
- **pdf-lib** v1.17.1 — client-side PDF merging
- **JSZip** v3.10.1 — client-side ZIP creation
- **jQuery** — from PESU Academy's page (not bundled)

## Privacy

ClawdMate runs entirely in your browser. It does not:

- Collect or transmit any personal data
- Send analytics or telemetry
- Communicate with any server other than `pesuacademy.com`
- Store data beyond the current page session

The extension only accesses `pesuacademy.com` using your existing session cookies.

## License

This project is licensed under the [MIT License](LICENSE). See [CONTRIBUTING](docs/CONTRIBUTING.md) to get involved.

## Author

**AAK1767**

Forked from [PESUmate](https://github.com/mohitpaddhariya/PESUmate) by Mohit Paddhariya.

## Acknowledgments

- Original project [PESUmate](https://github.com/mohitpaddhariya/PESUmate) by Mohit Paddhariya
- [pdf-lib](https://pdf-lib.js.org/) by Andrew Dillon
- [JSZip](https://stuk.github.io/jszip/) by Stuart Knightley
