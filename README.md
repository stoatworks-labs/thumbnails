# Stoatworks Labs — thumbnails

> **AI-assisted project.** The images here, and the documents describing them,
> were produced with [Claude](https://claude.com/claude-code) (Anthropic),
> directed and reviewed by a human author. Each one is a real frame or render
> from the project it belongs to, not an illustration of it.

Public image hosting for the [Stoatworks Labs](https://stoatworks-labs.com) project
videos. Nothing here is code.

## Why this repo exists

YouTube fetches a custom thumbnail **over the public internet, at upload time**. It
follows the URL once, from its own servers, with no credentials.

That means a thumbnail committed to the project's own repository only works if that
repository is **public**. For a private one, `raw.githubusercontent.com` returns 404,
YouTube quietly falls back to an auto-generated frame from the video, and the upload
API still reports success — `thumbnail_set: true` only means it tried. There is no
second chance: `youtubeThumbnailUrl` is an upload-time parameter, so the fix is a
re-upload under a new video ID.

Roughly half the fleet is private. So thumbnails live here instead, where the URL is
public regardless of what the project repo is doing.

## Layout

```
video/<slug>.png        1280x720, the YouTube thumbnail for that project's video
instagram/<cut>.jpg     1080x1920, the Instagram Reel cover (grid tile) for that cut
```

Instagram covers follow the same rule as YouTube thumbnails — `cover_url` is fetched
publicly at upload time and must be a JPEG — so covers for cuts of private-repo
projects live here too. `<cut>` is the cut's key in the video toolkit's
`social-captions.json`.

`<slug>` is the project's key in the website's `src/data/projects.json`.

## Using one

```
https://raw.githubusercontent.com/stoatworks-labs/thumbnails/main/video/<slug>.png
```

**Curl it for a `200` and an `image/png` before uploading.** A push takes a few seconds
to reach the raw CDN, and this is the only chance to get it right.

```bash
curl -sSI https://raw.githubusercontent.com/stoatworks-labs/thumbnails/main/video/arraycad.png | head -1
```

## Licence

The images are © Stoatworks Labs. They are published here so that YouTube can fetch
them, not as stock artwork.
