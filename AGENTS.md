# AGENTS.md — bringing an LLM up to speed on the thumbnails repo

Orientation for an AI assistant (or a new human) picking this repo up cold.

## What this is

**Public image hosting for the Stoatworks Labs project-video thumbnails.** Nothing here
is code, and nothing here is served to the website — the one consumer is **YouTube**, at
upload time. The [README](README.md) is the authority on why this repo exists and how to
use it; read it before touching anything.

## The three facts that matter

1. **YouTube fetches the thumbnail URL exactly once, at upload time, uncredentialed.**
   A thumbnail hosted in a *private* project repo 404s on `raw.githubusercontent.com`,
   YouTube silently falls back to an auto-frame, and the API still reports
   `thumbnail_set: true`. That silent failure is the entire reason this public repo
   exists — roughly half the fleet is private.

2. **There is no second chance.** The thumbnail URL is an upload-time parameter; fixing
   a bad one means re-uploading the video under a new video ID. Always
   `curl -sSI …/video/<slug>.png` for a `200 image/png` before any upload, and allow a
   few seconds after pushing for the raw CDN to catch up.

3. **`video/<slug>.png` is 1280×720 and `<slug>` must match the project's key in the
   website's `src/data/projects.json`.** The naming is load-bearing: upload tooling
   derives the URL from the slug.

## Conventions

- Thumbnails are *generated* (see `stoatworks-backend` and the website's
  `scripts/make_thumbnails.py` pipeline), not hand-drawn. Regenerate rather than edit
  a PNG in place.
- Committing here is publishing: the raw URL is public the moment it lands on `main`.
  Don't park drafts or private-project material here.
