# strictlyoptional.com

Personal tech blog. Hardware tuning, engineering deep-dives, data-driven analysis.

## Stack

- **Generator:** [Hugo](https://gohugo.io/) (Extended)
- **Theme:** [Blowfish](https://blowfish.page/) (git submodule)
- **Hosting:** GitHub Pages
- **CI:** GitHub Actions → `hugo --minify` → deploy

## Design System

- **Archetype:** Industrial / Utilitarian
- **Fonts:** JetBrains Mono (headings, data, code) + Inter (body)
- **Colors:** Blowfish scheme (slate neutrals, blue primary, cyan accent)
- **Custom CSS:** `assets/css/custom.css` — design tokens, no raw values

## Local Development

```bash
hugo server
```

Site runs at `http://localhost:1313/`.

## Project Structure

```
config/_default/     # Hugo + theme config (overrides only)
content/             # Markdown content
  posts/             # Blog articles
  about/             # About page
  projects/          # Projects page
layouts/
  partials/home/     # Custom homepage layout
  shortcodes/        # project card shortcode
assets/css/          # Design system (custom.css)
static/              # Favicons, CNAME
themes/blowfish/     # Theme (git submodule, don't edit)
```

## Adding Content

**New post:**
```bash
hugo new posts/my-post.md
```

**New project card** (in `content/projects/_index.md`):
```
{{</* project title="name" status="active" tech="Go, Python" link="https://..." */>}}
Description here.
{{</* /project */>}}
```

Status options: `active`, `complete`, `paused`.

## Deploy

Push to `main`. GitHub Actions builds and deploys automatically.
