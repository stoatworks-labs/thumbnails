# Stoatworks Labs — thumbnails

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
```

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
