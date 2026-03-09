# ClawdMate

A Chrome extension that adds bulk downloading of course materials from [PESU Academy](https://www.pesuacademy.com/Academy/). Download slides, notes, assignments, question banks, and question answers — merge PDFs, zip files, or download individually — right from the course page.

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

### From source (developer mode)

1. Clone this repository:
   ```bash
   git clone https://github.com/mohitpaddhariya/ClawdMate.git
   ```
2. Open Chrome and navigate to `chrome://extensions`
3. Enable **Developer mode** (toggle in the top-right corner)
4. Click **Load unpacked** and select the `ClawdMate` folder
5. The extension icon appears in your toolbar

### Prerequisites

- Google Chrome (or any Chromium-based browser)
- A valid PESU Academy student account

## Usage

1. Log in to [PESU Academy](https://www.pesuacademy.com/Academy/)
2. Navigate to **My Courses → [Your Subject] → Course Units**
3. Click the **ClawdMate** tab that appears at the end of the unit tabs
4. Select a resource type: **Slides**, **Notes**, **Assignments**, **QB**, or **QA**
5. Click **Merge & Download** to get everything, or click individual files

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
├── manifest.json       # Chrome extension manifest (MV3)
├── content.js          # Main content script — UI + fetch + merge logic
├── panel.css           # Styles for the download panel
├── popup.html          # Extension popup (toolbar icon click)
├── lib/
│   ├── pdf-lib.min.js  # PDF merging library (v1.17.1)
│   └── jszip.min.js    # ZIP creation library (v3.10.1)
├── icons/
│   ├── icon16.png
│   ├── icon48.png
│   └── icon128.png
├── LICENSE             # MIT License
└── docs/
    ├── CHANGELOG.md        # Release history
    ├── CODE_OF_CONDUCT.md  # Community standards
    ├── CONTRIBUTING.md     # Contribution guidelines
    ├── DEVELOPER.md        # Technical architecture docs
    └── SECURITY.md         # Security policy
```

## Tech stack

- **Manifest V3** — Chrome extension platform
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

**Mohit Paddhariya**

## Acknowledgments

- [pdf-lib](https://pdf-lib.js.org/) by Andrew Dillon
- [JSZip](https://stuk.github.io/jszip/) by Stuart Knightley
