<div align="center">

<img src="https://floxflix.github.io/floxflix-logo.png" alt="Floxflix" width="320" />

### Your video. Your server. Your rules.

A self-hosted video streaming platform. Upload source files, let FFmpeg transcode them to HLS in the background, and stream to your own users — with authenticated profiles and zero third-party cloud.

[![Website](https://img.shields.io/badge/website-floxflix.github.io-0b0b0f?style=flat-square)](https://floxflix.github.io/)
[![License](https://img.shields.io/badge/license-MIT-22c55e?style=flat-square)](https://github.com/floxflix/floxflix/blob/main/LICENSE)
[![Go](https://img.shields.io/badge/Go-1.26-00ADD8?style=flat-square&logo=go&logoColor=white)](https://github.com/floxflix/floxflix)
[![FFmpeg](https://img.shields.io/badge/FFmpeg-HLS-007808?style=flat-square&logo=ffmpeg&logoColor=white)](https://github.com/floxflix/floxflix)
[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?style=flat-square&logo=docker&logoColor=white)](https://github.com/floxflix/floxflix)

[**Get started**](https://github.com/floxflix/floxflix#readme) · [**API reference**](https://github.com/floxflix/floxflix/blob/main/openapi.yml) · [**Website**](https://floxflix.github.io/)

</div>

---

## What is Floxflix?

Floxflix is an open-source, self-hosted video streaming API written in Go. It ingests your own video files, transcodes them to adaptive HLS with FFmpeg, and serves them to authenticated clients — no vendor lock-in, no proprietary media server, no third-party cloud.

- **HLS streaming** — source files become `.m3u8` playlists with `.ts`, `.m4s` or `.mp4` segments, playable anywhere.
- **JWT authentication** — HTTP-only cookies plus bearer tokens, hashed passwords, revocable sessions.
- **Viewer profiles** — Netflix-style multi-profile support under a single account.
- **Upload & transcode** — multipart uploads for `mp4`, `mkv`, `mov`, `avi`, `webm` and more.
- **Background queue** — uploads return immediately while FFmpeg jobs process in a worker queue.
- **OpenAPI contract** — a complete `openapi.yml` and Postman collection ship with the project.

## Repositories

| Repository | Description | Stack |
| --- | --- | --- |
| [**floxflix**](https://github.com/floxflix/floxflix) | Self-hosted video streaming API — upload, transcode, authenticate, stream | Go · FFmpeg · MongoDB |
| [**floxflix-ui**](https://github.com/floxflix/floxflix-ui) | Admin dashboard and web client for the floxflix stack | TypeScript |
| [**floxflix-manifests**](https://github.com/floxflix/floxflix-manifests) | Raw Kubernetes manifests for the floxflix stack | Kubernetes |
| [**floxflix-telegram-bot**](https://github.com/floxflix/floxflix-telegram-bot) | Telegram bot for floxflix | Go |
| [**floxflix.github.io**](https://github.com/floxflix/floxflix.github.io) | Project website | HTML |

## Quickstart

```bash
# clone the API
git clone https://github.com/floxflix/floxflix
cd floxflix

# set the required secret
export JWT_SECRET_KEY=$(openssl rand -base64 48)

# start the API + MongoDB
docker compose up --build
```

The API listens on `http://localhost:7777`, with health checks at `/healthz` and routes under `/v1/api`.

```bash
# get a token (cookies are also set)
curl -X POST http://localhost:7777/v1/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "you@example.com", "password": "••••••••"}'

# upload a movie and kick off conversion
curl -X POST http://localhost:7777/v1/api/videos/upload \
  -H "Authorization: Bearer $TOKEN" \
  -F "file=@movie.mkv" \
  -F "type=movie" \
  -F 'values={"title":"Arrival","release_year":2016}'
```

## How it works

1. **Upload your source** — POST a video with its title, year, genre and cast. Movies and episodes are organized on disk automatically.
2. **FFmpeg transcodes it** — the background queue converts the source into HLS segments and a playlist, then marks the job complete.
3. **Stream to your users** — authenticated clients hit the streaming endpoint and play the HLS output in any modern browser, app or TV client.

## Community

- [Issues](https://github.com/floxflix/floxflix/issues) — bug reports and feature requests
- [Discussions](https://github.com/floxflix/floxflix/discussions) — questions and ideas
- [Contributing](https://github.com/floxflix/floxflix/blob/main/CONTRIBUTING.md) — how to get involved

---

<div align="center">

Released under the [MIT License](https://github.com/floxflix/floxflix/blob/main/LICENSE) · Built for people who host their own media.

<sub>hello@floxflix.io</sub>

</div>
