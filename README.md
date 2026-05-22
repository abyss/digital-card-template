# Mindreeder Consulting — Digital Business Card

A Hugo static site template for performer digital business cards. Deployable to any static hosting provider.

## Quick start

1. Clone this repo
2. Edit `data/card.yaml` with your details
3. Drop your photo in `static/photo.jpg`
4. Add any additional link icons to `assets/icons/` (SVGs from [Simple Icons](https://simpleicons.org/) work well)
5. Update `config/_default/config.toml` with your `baseURL` and `title`
6. Deploy anywhere that serves static sites (Cloudflare Pages, Netlify, GitHub Pages, etc.)

## Development

Requires [Hugo](https://gohugo.io/installation/) and [Task](https://taskfile.dev/installation/).

```sh
task dev    # dev server at http://localhost:8000
task build  # production build → public/
task clean  # remove build artifacts
```

## Themes

To switch themes, change the `theme` field in `data/card.yaml`:

```yaml
theme: minimal  # change this to any available theme name
```

Each theme is a layout partial + CSS in:

- `layouts/partials/themes/<name>/card.html`
- `assets/css/themes/<name>.css`

### Available themes

| Theme | Description | Key fields rendered |
|---|---|---|
| `minimal` | Clean, light card — white surface, subtle borders, Inter sans-serif | `name`/`displayName`, `jobTitle`, `company`, `subtitle`, `tagline`, `location`, `links`, `phone` |
| `merlin` | Parchment grimoire — torn-edge SVG scroll, starfield night sky, gold ink, Cinzel Decorative + EB Garamond | `name`/`displayName`, `tagline`, `location`, `links`, `phone` |
| `seance` | Ouija board — dark walnut grain, brass border, velvet background, Cormorant Garant italic name with amber glow | `name`/`displayName`, `tagline`, `location`, `links`, `phone` |
| `steam` | Steam locomotive — sooty near-black background, dark iron card, brass border, riveted corners, triple spinning gear divider, Oswald + Special Elite | `name`/`displayName`, `jobTitle`, `company`, `subtitle`, `location`, `links`, `phone` |
| `tabletop` | 19th century desktop blotter — dark mahogany table, dark velvet green felt card, aged brass triangular corner clasps, Playfair Display + IM Fell English | `name`/`displayName`, `jobTitle`, `company`, `subtitle`, `location`, `links`, `phone` |
| `honk` | Clown/circus performer — white card with vibrant polka dot background, rainbow photo ring, cycling bubblegum pill links, balloon watermarks, Boogaloo + Nunito | `name`/`displayName`, `jobTitle`, `company`, `subtitle`, `location`, `links`, `phone` |

### Adding a new theme

1. Create `layouts/partials/themes/<name>/card.html`
2. Create `assets/css/themes/<name>.css`
3. Set `theme: <name>` in `data/card.yaml`

Themes receive the full `card` data map as context. Use `{{ with .fieldName }}` guards for optional fields so the theme degrades gracefully when fields are absent.

## Data schema

All fields in `data/card.yaml` are optional except `name` and `theme`. See the file for field-level documentation.

### Phone formatting

North American numbers (NANP — `+1` prefix, 12 characters in E.164) are automatically formatted as `(NXX) NXX-XXXX`. All other numbers display as raw E.164. To change the display format, edit the `isNANP` branch in `layouts/index.html`.

### Phone obfuscation

The phone number is XOR-encoded so it doesn't appear as plaintext in the HTML source, defeating naive scrapers. Store it in E.164 format — the display formatting is handled client-side.

1. Generate the encoded array (replace with your number in E.164 format):
   ```sh
   node -e "console.log(JSON.stringify('+15550001234'.split('').map(c => c.charCodeAt(0) ^ 42)))"
   ```
2. Paste the output into `data/card.yaml` alongside the key:
   ```yaml
   phone: [25, 27, 10, ...]
   phoneKey: 42
   ```
3. The template decodes it at runtime via a small inline `<script>` — no library needed.

## Adding an icon

Drop an SVG file into `assets/icons/`. Use `fill="currentColor"` and a `viewBox="0 0 24 24"`. Reference it by filename (without `.svg`) as the `icon` field on any link.

## Deployment

Deploy anywhere that serves static files:

| Setting | Value |
|---|---|
| Build command | `hugo --minify` |
| Build output directory | `public` |
| Hugo version (env var) | `HUGO_VERSION = 0.145.0` (or latest) |

## License

[ISC License](LICENSE)
