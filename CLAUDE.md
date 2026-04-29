# Internal Resources & Training — Claude Code Guide

> Template: https://github.com/cathcoach4u/coach4u-shared/blob/main/templates/CLAUDE.md
> Shared design system: https://github.com/cathcoach4u/coach4u-shared
> Full setup guide: https://github.com/cathcoach4u/coach4u-shared/blob/main/SETUP.md

## Shared Stylesheet

Add to every HTML page `<head>`:

```html
<link rel="stylesheet" href="https://cathcoach4u.github.io/coach4u-shared/css/style.css">
```

## Supabase Project

| | |
|---|---|
| URL | `https://eekefsuaefgpqmjdyniy.supabase.co` |
| Anon Key | `sb_publishable_pcXHwQVMpvEojb4K3afEMw_RMvgZM-Y` |

## Critical Rules

**Supabase init — always inline.** GitHub Pages does not reliably load external `.js` modules. Always initialise Supabase inline in a `<script type="module">` block. Never import from an external config file.

**Reset password redirect.** Use `window.location.href` (not `window.location.origin`) when building the `redirectTo` URL. Using `origin` drops the path and breaks Supabase's redirect matching.

**Membership gating.** Every page except login/forgot/reset must verify `users.membership_status = 'active'` after confirming a session. Redirect to `inactive.html` if not.

## Auth Flow

- Login: email + password only (no magic link)
- Forgot password → `forgot-password.html`
- Reset password → `reset-password.html`

## Add a New Member (SQL)

```sql
INSERT INTO users (id, email, membership_status)
SELECT id, email, 'active'
FROM auth.users
WHERE LOWER(email) = LOWER('email@here.com');
```

---
## App-Specific Notes

**This is a MkDocs Material documentation site — it has no Supabase auth, no login pages, and no membership gating.** The shared rules above are included for reference alignment but do not apply to this repo.

### What this repo is

Training register, supervision records, and professional development hub for Coach4U coaches. Content covers:

- **Training Register** — master tracker for all certificates and CPD hours
- **Training** — organised by provider: Relate, Gottman, ADHD (PESI), Gallup, AIPC, MHPN, ICF
- **Supervision** — organised by supervisor: Marissa Clohesy, Relate

### Tech stack

- [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) — static site generator
- GitHub Actions — auto-deploys `main` to GitHub Pages on push
- No build step needed for content edits — just edit the Markdown files in `docs/`

### Local development

```bash
pip install -r requirements.txt
mkdocs serve
```

Then open http://localhost:8000.

### Deployed site

https://cathcoach4u.github.io/Internal-Resources-Training/

### Key files

| File | Purpose |
|------|--------|
| `mkdocs.yml` | Site config, nav structure, theme settings |
| `docs/` | All Markdown content pages |
| `docs/assets/stylesheets/extra.css` | Site-specific CSS overrides |
| `overrides/` | MkDocs Material theme overrides |
| `requirements.txt` | Python deps (`mkdocs-material`) |

### Adding content

1. Create a `.md` file in the appropriate `docs/` subfolder
2. Add the page to the `nav:` section of `mkdocs.yml`
3. Commit and push to `main` — GitHub Actions deploys automatically
