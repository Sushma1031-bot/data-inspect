# Viewfinder — File Metadata Inspector

A single-file, client-side web app that extracts and displays metadata from images and PDFs. Nothing is uploaded anywhere — all parsing happens in the browser.

## Features

**All files (via the Browser File API)**
- File name, size, MIME type
- Last-modified date

**Images only**
- Width, height, aspect ratio, megapixels (via `Image.naturalWidth` / `naturalHeight`)
- Live thumbnail preview
- Camera metadata via EXIF.js: make, model, lens, aperture, shutter speed, ISO, focal length, orientation
- Date taken (from EXIF `DateTimeOriginal`)
- GPS latitude/longitude/altitude (converted from EXIF DMS to decimal degrees), with a link to view the location on a map

**PDFs**
- Size, type, and modified date (same as any file)
- A clear note explaining that page count/author/producer live inside the PDF's internal structure and aren't exposed by the Browser File API

**Robustness**
- If EXIF.js fails to load (offline, blocked script, ad-blocker), the app degrades gracefully: the photo preview and all File-API metadata still work, and the camera/GPS panel shows an "unavailable" message instead of breaking.
- Unsupported file types show a clear inline error instead of failing silently.

## Tech stack

- Plain HTML/CSS/JS — no build step, no framework, no server
- [EXIF.js](https://github.com/exif-js/exif-js) (v2.3.0), loaded from `cdnjs.cloudflare.com`
- Fonts: Oswald (display), Inter (body), JetBrains Mono (data readouts), loaded from Google Fonts

## Running it

Just open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari). No install, no server required.

```
open index.html
```

or double-click the file / drag it into a browser window.

> Requires an internet connection on first load, to fetch EXIF.js and the web fonts from their CDNs.

## Usage

1. Click the frame, or drag a file onto it.
2. Supported types: JPEG, PNG, WEBP, GIF, and PDF.
3. Metadata populates automatically across four panels: **File**, **Dimensions**, **Timestamps**, and **Camera / EXIF** (plus **GPS** for geotagged photos).

## Known limitations

- **True file creation date** isn't exposed by browsers — only "last modified" is available via the File API. The app is explicit about this rather than guessing.
- **HEIC/HEIF photos** (the default format on many iPhones) can't be decoded by `<img>` in most browsers, so no thumbnail or EXIF will show for those files. Convert to JPEG first, or the phone's share sheet usually offers a JPEG export option.
- **EXIF only applies to JPEG/TIFF** by the spec. PNGs, WEBPs, screenshots, and images re-exported by messaging apps or social platforms typically have EXIF stripped — the app reports "None found" rather than treating this as an error.
- **PDF page dimensions/page count** are not read; doing so would require parsing the PDF's internal object structure (e.g. with a library like `pdf.js`), which is outside the current scope (File-API + EXIF.js only, per the project brief).

## Project structure

```
index.html   — everything: markup, styles, and logic in one file
README.md    — this file
```

## Privacy

Files are read entirely client-side using `FileReader` / `URL.createObjectURL`. Nothing is sent to a server at any point.
