# celevion-site

Static "About" site for **Celevion**, an independent studio founded by Loïc Cantin in France,
making small offline single-player mobile games.

Two pages, no build step, no dependency:

| Path | Language |
|---|---|
| `index.html` | English |
| `fr/index.html` | French |

`logo.svg` (512 × 512, opaque background, mark + wordmark) is the profile picture for TikTok,
Google Play and YouTube; it stays readable inside a circular crop. The wordmark is Lato Heavy
(SIL Open Font License) converted to outlines, so no font is needed to display it.

Art direction: *a pocket sky chart* — night ink on blue-tinted paper, an orbit drawn in one
stroke where each game is a small, complete body. The mark is that open orbit with the ball
that completes it. Palette in CSS variables, light and dark (`prefers-color-scheme`); one
CSS-only entrance on load, removed under `prefers-reduced-motion`.

Both pages carry their CSS inline. There is no JavaScript, no external font, no tracker and
no cookie anywhere on the site. `CNAME` holds the custom domain `celevion.com`.

## Local preview

```sh
python3 -m http.server 8000   # then open http://localhost:8000/ and http://localhost:8000/fr/
```

## Publication steps (owner)

1. **Create the GitHub repository** — `caissatech/celevion-site` (public), then push this
   branch `main` to it:
   ```sh
   git remote add origin https://github.com/caissatech/celevion-site.git
   git push -u origin main
   ```
2. **Enable GitHub Pages** — repository *Settings → Pages → Build and deployment → Source:
   Deploy from a branch*, branch `main`, folder `/ (root)`. The `CNAME` file is what tells
   Pages the custom domain. After the first build, turn on *Enforce HTTPS*.
3. **DNS at OVH** — zone `celevion.com`, *Zone DNS → records*:

   | Type | Sub-domain | Target | TTL |
   |---|---|---|---|
   | A | `@` (apex) | `185.199.108.153` | 3600 |
   | A | `@` (apex) | `185.199.109.153` | 3600 |
   | A | `@` (apex) | `185.199.110.153` | 3600 |
   | A | `@` (apex) | `185.199.111.153` | 3600 |
   | CNAME | `www` | `caissatech.github.io` | 3600 |

   These four A records and the `www` CNAME are the addresses GitHub Pages documents for
   custom domains. Delete any conflicting `A`/`AAAA`/`CNAME` record on `@` or `www`.
4. **Redirect `celevion.eu`** — OVH, zone `celevion.eu`: *Redirection → 301 toward*
   `https://celevion.com` (apex and all paths), so the old domain lands on the studio page.
5. **Mail redirection** — OVH, *Redirection e-mail*: `contact@celevion.com` → the real
   mailbox to read. `contact@celevion.com` is the only address published on the site.
6. **Check** — once DNS has propagated:
   ```sh
   dig +short celevion.com          # expect the four GitHub Pages IPs
   dig +short www.celevion.com      # expect caissatech.github.io
   curl -sI https://celevion.com/   # expect HTTP 200
   curl -sI https://celevion.com/fr/
   ```

## Checks run on this content

- HTML validation: `html-validate` (npm), no error on either page.
- Rendering: headless Chrome screenshots at 360, 1280 and 1920 px, light and dark, no console
  error, no external request, no horizontal overflow.
- No external request: no `http(s)` asset, no `<script>`, no `@font-face`.
