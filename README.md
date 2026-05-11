# Catppuccin for Nextcloud

Soothing pastel theme for [Nextcloud](https://nextcloud.com), built against the **Nextcloud 32** CSS variable schema.

- **Dark mode** → Catppuccin Mocha
- **Light mode** → Catppuccin Latte
- Switches automatically based on your OS/browser preference (`prefers-color-scheme`)

> **Compatibility:** Nextcloud 32+. The variable names changed significantly in NC32 — this theme will not work on older versions.

---

## Installation

### Prerequisites

Enable the **Custom CSS** app from the Nextcloud App Store (Apps → Search "Custom CSS" → Enable), or via CLI:

```bash
sudo docker exec --user www-data nextcloud-aio-nextcloud php occ app:install theming_customcss
```

### Option A — Admin UI

1. Log in as admin → **Settings → Administration → Theming**
2. Scroll to the **Custom CSS** field
3. Paste the full contents of [`catppuccin.css`](catppuccin.css)
4. Save

### Option B — CLI (Docker AIO)

```bash
CSS=$(cat catppuccin.css)
sudo docker exec --user www-data nextcloud-aio-nextcloud \
  php occ config:app:set theming_customcss customcss --value="$CSS"

# Bump cachebuster so browsers reload the stylesheet
sudo docker exec --user www-data nextcloud-aio-nextcloud \
  php occ config:app:set theming_customcss cachebuster --value='1'
```

### Recommended admin theming settings

Set these in **Settings → Administration → Theming** (or via `occ`) for the best result:

| Setting | Value | Notes |
|---|---|---|
| Primary color | `#cba6f7` | Mocha Mauve — used for buttons and accents |
| Background color | `#1e1e2e` | Mocha Base — header fallback color |
| Background image | None (plain color) | Remove the default Nextcloud wallpaper |

Via CLI:

```bash
sudo docker exec --user www-data nextcloud-aio-nextcloud php occ config:app:set theming primary_color --value='#cba6f7'
sudo docker exec --user www-data nextcloud-aio-nextcloud php occ config:app:set theming background_color --value='#1e1e2e'
sudo docker exec --user www-data nextcloud-aio-nextcloud php occ config:app:set theming backgroundMime --value='backgroundColor'
sudo docker exec --user www-data nextcloud-aio-nextcloud php occ config:app:set theming cachebuster --value='1'
```

---

## Palette

### Mocha (dark)

| Role | Color | Hex |
|---|---|---|
| Background | Base | `#1e1e2e` |
| Header | Mantle | `#181825` |
| Surface | Surface0 | `#313244` |
| Text | Text | `#cdd6f4` |
| Accent | Mauve | `#cba6f7` |
| Error | Red | `#f38ba8` |
| Success | Green | `#a6e3a1` |
| Warning | Yellow | `#f9e2af` |
| Info | Blue | `#89b4fa` |

### Latte (light)

| Role | Color | Hex |
|---|---|---|
| Background | Base | `#eff1f5` |
| Header | Surface0 | `#e6e9ef` |
| Surface | Surface1 | `#ccd0da` |
| Text | Text | `#4c4f69` |
| Accent | Mauve | `#8839ef` |
| Error | Red | `#d20f39` |
| Success | Green | `#40a02b` |
| Warning | Yellow | `#df8e1d` |
| Info | Blue | `#1e66f5` |

---

## How it works

The **Custom CSS** app (`theming_customcss`) injects a stylesheet after all of Nextcloud's own theme CSS files. This means `:root` variable overrides here win over the defaults without touching any core files — the theme survives Nextcloud updates cleanly.

The CSS uses two `@media (prefers-color-scheme: ...)` blocks so it respects your system preference automatically. No Nextcloud appearance setting needs to be changed.

---

## Prior art

Inspired by [Motschen/nextcloud](https://github.com/Motschen/nextcloud). This is a full rewrite targeting the NC32 variable schema, which changed significantly from earlier versions.
