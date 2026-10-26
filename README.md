<p align="center">
  <a href="https://secondread.cc">
    <img src="https://www.secondread.cc/og-image.png" alt="Second Read - your saved highlights, on every new tab" width="100%" />
  </a>
</p>

# Second Read

**A Chrome extension that shows you one of your saved highlights on every new tab.**

[Website](https://secondread.cc) · [Chrome Web Store](https://chromewebstore.google.com/detail/cmangconocmmfncdaicpcijnoaagfamp) · [Telegram bot](https://t.me/secondreadtool_bot)

> This is a showcase repository. The source code is private. Happy to walk through it on a call: [cal.com/waris-hussain](https://cal.com/waris-hussain)

---

## The problem

Most highlighting tools can only save text you can select. Slides, images, video frames, scanned PDFs and Kindle Cloud Reader end up as screenshots nobody looks at again. And even the highlights that do get saved pile up in an archive nobody opens, so they're never read a second time.

Second Read captures any text on screen and puts it back in front of you, one highlight per new tab.

## Who it's for

People who read a lot across the web, Kindle and PDFs, save highlights, and want to actually come back to them.

## Features

- **Capture anything.** Press `Cmd/Ctrl+Shift+S` and drag a box around any text on screen, including text you can't select.
- **One highlight per new tab**, picked by how long it has gone unseen.
- **Kindle sync** imports your highlights book by book and checks for new ones twice a day.
- **Import and export.** Bring highlights in from Readwise (CSV) or JSON. Export to Markdown or JSON.
- **Digests** of 5 highlights by email or Telegram, on a schedule you choose.
- **Telegram bot.** Send a passage or a tweet link to save it.
- **Web library** with search, filters, tags, archive and editing.

## How it works

1. Install the Chrome extension.
2. Select text, or drag a box around anything on screen. It saves with its source.
3. Optionally connect Kindle or import from Readwise.
4. Open a new tab and one saved highlight appears.
5. Browse, edit and export everything at [secondread.cc](https://secondread.cc).

## Tech stack

| Layer | Tools |
| --- | --- |
| Extension | Chrome Manifest V3, TypeScript, Vite, no UI framework |
| Website | Next.js 16, React 19, Tailwind CSS 4, Vercel |
| Backend | Supabase: Postgres with Row Level Security, email-code and Google sign-in, Edge Functions on Deno, pg_cron scheduled jobs |
| OCR | Tesseract.js (WebAssembly), running in the browser |
| Services | Resend (email), Telegram Bot API |

## Engineering highlights

- **On-device OCR, nothing uploaded.** Chrome extensions can't run remote code, so Tesseract, its language data and fonts ship inside the package. Chrome's extension rules split the capture across four parts: the page overlay, a background worker, a hidden helper page and the popup.
- **Offline first.** Captures save to the device and sync in the background. The new tab reads only local storage, so it opens instantly.
- **No duplicate imports.** Each imported highlight's ID is derived from its own text (UUIDv5), so re-running an import never creates duplicates.
- **Reliable Kindle import without an API.** Amazon has no highlights API. The importer requests each book by ID and verifies the response matches before saving.
- **One ranking formula everywhere.** The new tab, popup, web dashboard and digests share the same logic, so every surface agrees on what's due.
- **Privacy by design.** The Kindle permission is optional and limited to read.amazon.com, every database table only exposes a user's own rows, and the site sends strict security headers including a Content-Security-Policy.

## Privacy

Text in images is read on your device and the image is discarded, so nothing is uploaded. If you're signed in, your saved text, its source, tags and email are stored in a database where each account can only access its own rows. If you're not signed in, everything stays on your device.

## Status

**Live** on the Chrome Web Store. Development started in August 2026; version 1.0.0 was submitted eight days after the first commit. Current version: 1.3.2.

- 20 tagged releases, 9 database migrations
- About 16,000 lines of TypeScript, SQL and CSS across the extension, website and backend

## Built by

**[Waris Hussain](https://www.warishussain.com)**, founder and sole operator: product, design, brand, copy, marketing and release management. Built with Claude Code as the engineering tool.

[Website](https://www.warishussain.com) · [LinkedIn](https://www.linkedin.com/in/waris-hussain/) · [X](https://x.com/WarisHussain57) · [GitHub](https://github.com/wrxwrxsti)
