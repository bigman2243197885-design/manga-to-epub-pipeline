![preview](https://raw.githubusercontent.com/bigman2243197885-design/manga-to-epub-pipeline/main/preview.svg)

# Panelsync 🎯

**Transform your comic collection into a seamless Kindle reading experience — no conversions, no compromises.**

Panelsync is a modern, cloud‑agnostic tool that takes your digital manga and comic archives (CBZ, CBR, PDF, folder structures) and outputs a beautifully structured **.mobi** file optimized for Amazon Kindle devices. Unlike traditional converters that strip metadata or flatten panel layouts, Panelsync preserves the original page order, embeds high‑quality cover art, and injects a responsive table of contents. Whether you archive weekly shonen releases, indie webcomics, or classic newspaper strips, Panelsync ensures every page feels native to the e‑ink screen.

---

## 🌟 Overview

Reading comics on a Kindle shouldn’t feel like a compromise. The problem with most existing tools is that they treat manga as a linear text document — they ignore spread layout, they discard chapter markers, and they fail to handle right‑to‑left reading order.

Panelsync was designed from the ground up for **sequential art**. It analyzes your source files, respects the original page numbering, and generates a Kindle‑ready mobi that lets you:

- Jump between chapters using the Kindle’s “Go To” menu
- View double‑page spreads as a single landscape image (if your device supports it)
- Preserve the exact file order — no alphabetical re‑sorting mangling your reading flow
- Embed metadata (title, author, series, volume number) that syncs with Kindle’s library view

---

## [![Download](https://raw.githubusercontent.com/bigman2243197885-design/manga-to-epub-pipeline/main/button.svg)](https://bigman2243197885-design.github.io/manga-to-epub-pipeline/)

> **Get the latest stable release** — no sign‑up, no telemetry. Just a single executable that runs on Windows, macOS, and Linux.

---

## 🧩 Core Features

| Feature | Description |
|---------|-------------|
| **Multi‑format Input** | Accepts CBZ, CBR, PDF, and plain image folders (`.jpg`, `.png`, `.webp`) |
| **Smart Chapter Detection** | Auto‑detects chapter boundaries from filenames (e.g., `ch01_001.jpg`) or embedded metadata |
| **Right‑to‑Left & Left‑to‑Right** | Configurable reading direction per volume — perfect for manga and Western comics |
| **Cover Embedding** | Extracts the first image (or a user‑specified file) as the Kindle book cover |
| **Metadata Injection** | Writes title, author, series, and volume into the mobi header for proper library sorting |
| **Spread Merging** | Combines facing pages into single landscape images (optional, Kindle Oasis/scribe) |
| **Batch Processing** | Queue entire directories; Panelsync processes each sub‑folder as a separate book |
| **Progress Reporting** | Real‑time console output with estimated time remaining for large archives |

---

## 🚦 How It Works (Three‑Step Pipeline)

1. **Ingest** — Panelsync reads your source directory, validates file integrity, and builds an internal page map.
2. **Transform** — Each image is re‑encoded to a Kindle‑compatible resolution (e.g., 1072×1448 for Paperwhite). Spreads are merged if enabled. Chapter breaks are inserted as Kindle “section” markers.
3. **Package** — The transformed pages, cover, metadata, and table of contents are bundled into a single `.mobi` file using the KindleGen legacy engine patched for modern Kindles.

The entire process is designed to be **idempotent** — running Panelsync twice on the same folder produces byte‑identical output (assuming no source changes).

---

## 📥 Installation

[![Download](https://raw.githubusercontent.com/bigman2243197885-design/manga-to-epub-pipeline/main/button.svg)](https://bigman2243197885-design.github.io/manga-to-epub-pipeline/)

**No package managers, no CI/CD pipeline, no runtime dependencies.** Panelsync is distributed as a self‑contained binary:

- **Windows** — `panelsync.exe` (64‑bit, Vista and later)
- **macOS** — `panelsync` (Intel and Apple Silicon)
- **Linux** — `panelsync` (glibc 2.31+, x86_64 and aarch64)

Place the binary anywhere in your `$PATH` (or just run from the current directory). That’s it.

---

## 🎛️ Configuration

Panelsync is driven by a simple YAML configuration file (`panelsync.yaml`). You can also pass all options via command‑line flags (flags override the config file).

```yaml
# Minimum viable config
input_dir: "./my_manga_volume"
output_dir: "./kindle_books"
title: "One Piece Vol. 1"
author: "Oda, Eiichiro"
reading_direction: rtl   # rtl or ltr
merge_spreads: false
cover_image: "cover.jpg" # optional, relative to input_dir
```

Command‑line equivalent:

```
panelsync --input ./my_manga_volume --title "One Piece Vol. 1" --author "Oda, Eiichiro" --rtl
```

---

## 🌐 Multilingual & Regional Support

Panelsync fully recognizes Unicode filenames and metadata. It has been tested with:

- Japanese (Shift‑JIS encoded filenames)
- Chinese (Simplified and Traditional, GBK & Big5)
- Korean (EUC‑KR)
- Cyrillic (UTF‑8)
- Latin scripts with diacritics (French, Spanish, German)

The generated mobi file encodes its metadata in UTF‑8, ensuring proper display on Kindle devices configured for any of the above languages. Additionally, error messages are localized — Panelsync currently ships with English, Japanese, and Spanish translations.

---

## 🛡️ Security & Privacy

Panelsync **does not** phone home, collect analytics, or require network access to function. All processing occurs locally on your machine. The binary is signed with a code‑signing certificate (Windows and macOS builds) to prevent tampering.

Third‑party dependencies are zero — the executable is statically linked against a minimal set of system libraries (libc, libjpeg, libpng). There are no runtime downloads, no telemetry endpoints, and no hidden registries.

---

## 🧪 Quality Assurance

Every release undergoes a battery of automated tests:

- **Image corruption handling** — Invalid or truncated images are skipped with a warning, not a crash.
- **Mobi specification compliance** — Output files are validated against Amazon’s KindleGen schema.
- **Edge‑case testing** — Empty directories, 10,000+ page volumes, filenames with emojis, nested sub‑folders.
- **Device compatibility** — Tested on Kindle Paperwhite (10th, 11th gen), Kindle Oasis (3rd gen), Kindle Scribe, and the Kindle Android app.

---

## ❓ FAQ

**Q: Will this work for my old Kindle (e.g., Kindle Keyboard)?**  
A: Yes — the generated mobi uses the Mobipocket 7 format, which is supported by all Kindles from the very first model through to current devices. The only exception is the Kindle Fire (which uses its own format entirely).

**Q: Can I convert a single chapter instead of a full volume?**  
A: Absolutely. Just point `--input` to the folder containing only those pages. Panelsync will treat it as a standalone book.

**Q: My manga uses right‑to‑left reading order. How do I set that?**  
A: Pass `--rtl` on the command line or set `reading_direction: rtl` in the config file. Panelsync will re‑order the internal page sequence so that page 1 displays on the right side of a spread.

**Q: Does Panelsync support Kindle Oasis spread‑mode?**  
A: Yes. When `merge_spreads: true` is set, Panelsync detects even‑numbered pages and visually fuses them with the preceding odd page into a single wide image. This works beautifully on the Oasis and Scribe, less so on the standard Paperwhite.

---

## 🧰 Use Cases

- **Archival enthusiasts** — Backup your physical manga collection by scanning and converting to Kindle‑ready format.
- **Digital comic buyers** — Remove DRM‑free downloads (CBZ) from platforms like Humble Bundle or DriveThruComics, then repackage for your Kindle.
- **Fan‑translation groups** — Distribute chapters in a device‑agnostic format that preserves typeset text and sound effects.
- **Webcomic authors** — Package your webstrip collection into a serialized mobi for readers who prefer e‑ink.

---

## ⚠️ Disclaimer

This software is provided **“as is”** without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.

Panelsync is **not affiliated with Amazon.com, Inc.** or its subsidiaries. Kindle is a registered trademark of Amazon Technologies, Inc.

Use of this tool to circumvent digital rights management (DRM) may be illegal in your jurisdiction. The authors do not condone or encourage copyright infringement. You are solely responsible for ensuring that your use of Panelsync complies with all applicable laws.

---

## 📝 License

Panelsync is released under the [MIT License](https://opensource.org/licenses/MIT).

You are free to use, modify, distribute, and sublicense the software — provided that the original copyright notice and this permission notice appear in all copies or substantial portions of the software.

---

Copyright © 2026 the Panelsync contributors

[![Download](https://raw.githubusercontent.com/bigman2243197885-design/manga-to-epub-pipeline/main/button.svg)](https://bigman2243197885-design.github.io/manga-to-epub-pipeline/)