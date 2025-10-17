# Copilot instructions for Infinity-/adalysis
- This repository is a static mirror of https://adalysis.com generated from WordPress HTML; there is no backend or build.
- Two mirrors exist: `public/` (deploy target with relative assets) and `adalysis.com/` (raw crawl). Update content under `public/` unless you are refreshing the crawl.

## Page layout conventions
- Every page is pre-rendered HTML with WP Rocket lazy-loading wrappers (`rocketlazyloadscript`, inline JSON). Leave those attributes untouched so deferred scripts keep working.
- Navigation, footer, and hero modules are copy-pasted per file; pages live either at `public/<slug>/index.html` or `public/index.html?p=<id>.html`. Preserve those filenames so existing inbound links resolve.
- Global assets live beneath `public/wp-content/`; fonts are copied locally while CSS/JS are cached in `public/wp-content/cache/...`. Prefer injecting small page-specific styles inline near the closing `</head>` instead of trying to rebuild the minified cache.
- Forms and CTAs call back to `https://adalysis.com/wp-admin/admin-ajax.php`; keep those endpoints and hidden inputs intact so submissions continue to post to the live WordPress backend.

## Working with assets
- When adding images, drop them inside `public/wp-content/uploads/<year>/<month>/` to match existing structure and reference with relative `../wp-content/uploads/...` URLs.
- For new SVG icons, check existing ones under `public/wp-content/themes/adalysis/assets/img/content/` and reuse the naming scheme (kebab-case, no spaces).
- Any third-party script, iframe, or font must be whitelisted in both `vercel.json` files (root and `public/vercel.json`) to satisfy the project-wide Content-Security-Policy.

## Previewing and validating
- Run a static preview by serving `public/`, e.g. `npx serve public` or `python3 -m http.server 8080` from that folder; verify relative links and assets still resolve.
- There are no automated tests or builds, so spot-check modified pages in a browser and view-source to ensure you did not accidentally reformat critical minified sections.
- Keep diff noise low: avoid prettier-style rewrites, respect existing indentation, and limit changes to the minimal snippet you intend to edit.

## Frequent edits
- For copy updates, search-and-replace across `public/` but double-check meta tags (many still mention both “Adalysis” and “Waduit”) before shipping.
- To adjust navigation or footer links, edit one representative page, then propagate the same snippet to other affected HTML files; there is no shared include.
- Blog post pages live under `public/blog/<slug>/index.html`; check the open graph metadata near the top when updating headlines or hero images.

## Deployment awareness
- Vercel deploys directly from the repository head; no build step runs, so ensure every asset you reference is committed.
- Security headers inherit from `vercel.json`; if you add a new subresource (e.g., YouTube embed), update `script-src`, `style-src`, or `frame-src` accordingly and mirror the change in both copies of the file.
- Large binary assets can bloat deploys—prefer compressing WebP or reusing existing graphics where possible.

## Support tips
- If you need fresh HTML from WordPress, drop the raw export into `adalysis.com/` first, then adapt it into `public/` by fixing relative paths and ensuring shared assets exist locally.
- Note that some mirrors reference remote CDN URLs; if you change domains, adjust both canonical tags and social meta tags to avoid regressions.
- When in doubt, compare against the same page under `adalysis.com/` to see how the original markup behaves before editing `public/`.
