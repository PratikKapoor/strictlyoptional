# strictlyoptional.com

Personal tech blog. Hardware tuning, engineering deep-dives, data-driven analysis.

## Stack

- **Generator:** [Hugo](https://gohugo.io/) (Extended)
- **Theme:** [Blowfish](https://blowfish.page/) (git submodule)
- **Hosting:** GitHub Pages
- **CI:** GitHub Actions → build → htmltest → Lighthouse CI → deploy
- **Testing:** HTML validation, broken link checking, performance/accessibility/SEO audits

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
.htmltest.yml        # HTML validation + link checking config
lighthouserc.json    # Lighthouse CI thresholds + page URLs
themes/blowfish/     # Theme (git submodule, don't edit)
```

## Adding Content

**New post:**
```bash
hugo new posts/my-post.md
```

**New page** (e.g. `/uses/`):
1. Create `content/uses/_index.md` with front matter
2. Add the page URL to `lighthouserc.json` → `ci.collect.url` array
3. Optionally add a nav link in `config/_default/menus.en.toml`

**New project card** (in `content/projects/_index.md`):
```
{{</* project title="name" status="active" tech="Go, Python" link="https://..." */>}}
Description here.
{{</* /project */>}}
```

Status options: `active`, `complete`, `paused`.

## CI/CD Pipeline

Every push to `main` and every PR runs:

| Step | Tool | What it checks |
|------|------|---------------|
| Build | Hugo | Template errors, missing refs |
| HTML | htmltest | Broken internal links, missing images, HTTPS |
| Audit | Lighthouse CI | Performance ≥ 0.8, accessibility ≥ 0.9, SEO ≥ 0.9 |
| Deploy | gh-pages | Only on push to `main` (skipped for PRs) |

Config files: `.htmltest.yml`, `lighthouserc.json`, `.github/workflows/hugo.yml`

### Checklist for new pages

When adding a new page, remember to:
- [ ] Add the URL to `lighthouserc.json` `url` array so Lighthouse audits it
- [ ] Verify `htmltest` passes locally: `hugo --minify && htmltest -c .htmltest.yml`

## Deploy

Push to `main`. GitHub Actions builds and deploys automatically.
