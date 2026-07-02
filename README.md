# fulgur_deploy

Orchestrates the gk-fulgur.ru stack (static site, API, admin panel) behind Caddy on a single VPS.

## Layout

Clone all four repos side by side:

```
fulgur_admin/
fulgur_dotnet/
fulgur_solders/
fulgur_deploy/   <- this repo, run docker compose from here
```

## Setup

1. Point DNS `A` records for `gk-fulgur.ru` and `admin.gk-fulgur.ru` at the VPS.
2. `cp .env.example .env` and fill in `AUTH_SECRET_KEY` (any long random string) and `SMTP_PASSWORD`.
3. `docker compose up -d --build`

Caddy automatically requests and renews Let's Encrypt certificates for both domains on first request.

## Routing

- `gk-fulgur.ru/` -> fulgur_solders static site
- `gk-fulgur.ru/api/*` -> fulgur_dotnet API
- `admin.gk-fulgur.ru/` -> fulgur_admin panel
- `admin.gk-fulgur.ru/api/*` -> same fulgur_dotnet API

Uploaded product images are written by the API into the `images-data` volume, which the fulgur_solders
container also mounts at `/usr/share/nginx/html/images`, so they're immediately served at
`gk-fulgur.ru/images/<filename>`.
