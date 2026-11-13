# Happy Hiker Properties — "Coming Soon" Site Plan

## Goal

Publish a single-page "coming soon" placeholder at **https://happyhikerproperties.com**, hosted free on GitHub Pages from the `nacler-83/happyhikers` repo. The page should feel warm and outdoorsy, with a cabin in the woods as the main image, and leave room to grow into a rental listing site later.

## Scope (v1)

In scope:
- One responsive landing page (works on desktop and phone)
- Full-screen hero image of a cabin in the woods
- Brand name, short tagline, and a "Coming Soon" message
- Custom domain + HTTPS through GitHub Pages
- Basic SEO and social-share metadata (title, description, Open Graph image)
- Favicon

Out of scope for now (later phases):
- Property listing, photo gallery, amenities, pricing
- Booking calendar or availability
- Email signup form that needs a backend
- Analytics

## Design

**Layout:** A full-viewport hero photo with a dark gradient overlay so the text stays readable. Content is centered:

```
┌─────────────────────────────────────────┐
│        [cabin-in-the-woods photo]       │
│                                         │
│           HAPPY HIKER PROPERTIES        │
│     Your cabin in the woods awaits.     │
│                                         │
│              — Coming Soon —            │
│                                         │
│                                         │
│                                         │
│   © 2026 Happy Hiker · Photo: [credit]  │
└─────────────────────────────────────────┘
```

**Look and feel:**
- Palette: forest green (`#2F4F3A`), warm cream (`#F5EFE6`), and a wood/amber accent (`#C08A4B`)
- Type: a serif display font for the name (e.g. *Playfair Display* or *Lora*) and a clean sans-serif for body text (e.g. *Inter*), loaded from Google Fonts
- A small hiking or pine-tree line icon next to the name (inline SVG, no image file needed)
- A slow, subtle zoom on the hero image (turned off for users who prefer reduced motion)

**Imagery:**
- **Decision: Option A (stock photo for launch).**
- **Option A:** A free-license stock photo from Unsplash or Pexels (search "cabin woods", "forest cabin"). Both licenses allow commercial use without attribution, though we'll add a small photo credit anyway. The image is downloaded and stored in the repo, not hot-linked.
- **Option B:** A photo of your actual property. Swap it in whenever you're ready; it's a one-file change.
- The image will be compressed and served as WebP with a JPEG fallback, around 1920px wide and under ~400 KB, so the page loads fast.

## File structure

No build tools or frameworks. Plain static files that GitHub Pages serves directly:

```
happyhikers/
├── index.html          # the page
├── styles.css          # styling
├── CNAME               # contains: happyhikerproperties.com
├── 404.html            # simple "not found" page that links home
├── robots.txt
├── assets/
│   ├── cabin-hero.webp
│   ├── cabin-hero.jpg  # fallback
│   ├── og-image.jpg    # 1200×630 social-share preview
│   └── favicon.svg
├── plan.md             # this file
└── README.md           # how to update and deploy
```

## Implementation steps

1. **Build the page:** create `index.html`, `styles.css`, `404.html`, `robots.txt`, and the favicon.
2. **Add imagery:** pick and download the cabin photo, then compress it and make the WebP, JPEG, and social-share versions.
3. **Preview locally:** run `python3 -m http.server` and check desktop and phone widths.
4. **Commit and push** to `main` on `nacler-83/happyhikers`.
5. **Turn on GitHub Pages:** Repo → Settings → Pages → Source: "Deploy from a branch", branch `main`, folder `/ (root)`.
6. **Set the custom domain:** add the `CNAME` file and enter `happyhikerproperties.com` in Settings → Pages → Custom domain.
7. **Configure DNS at GoDaddy** (see below).
8. **Enforce HTTPS:** once DNS resolves and GitHub issues the certificate (minutes to about 24 hours), check "Enforce HTTPS".
9. **(Recommended) Verify the domain** in GitHub account Settings → Pages → "Add a domain". This stops anyone else from claiming it on GitHub Pages.

## DNS records (GoDaddy)

Apex domain (`happyhikerproperties.com`), with four **A** records:

| Type | Host | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

Optional IPv6 (**AAAA**) records:

| Type | Host | Value |
|------|------|-------|
| AAAA | @ | 2606:50c0:8000::153 |
| AAAA | @ | 2606:50c0:8001::153 |
| AAAA | @ | 2606:50c0:8002::153 |
| AAAA | @ | 2606:50c0:8003::153 |

`www` subdomain:

| Type | Host | Value |
|------|------|-------|
| CNAME | www | nacler-83.github.io |

With both set up, GitHub automatically redirects `www.happyhikerproperties.com` to the apex domain.

Remove any existing "parking page" or URL-forwarding records the registrar added, because they will conflict with these.

### Step-by-step in GoDaddy

1. Sign in at godaddy.com → profile menu → **My Products** → next to `happyhikerproperties.com` click **DNS** (or **Manage DNS**).
2. On the **DNS Records** tab, look for an existing **A** record with Name `@` (often pointing to "Parked" or a GoDaddy IP). **Edit** it to `185.199.108.153`.
3. Click **Add New Record** three more times to add the other three A records (`@` → `185.199.109.153`, `.110.153`, `.111.153`). Leave TTL at the default (1 hour).
4. (Optional) Add the four **AAAA** records for `@` listed above.
5. Find the existing **CNAME** record with Name `www` (GoDaddy usually points it to `@` or `happyhikerproperties.com`). **Edit** its value to `nacler-83.github.io`.
6. Go to the **Forwarding** tab (or "Domain forwarding" section). If any forwarding is set for the domain or `www`, **delete** it.
7. If GoDaddy shows a **Website / Websites + Marketing** builder or "parked page" connected to the domain, disconnect it. It can override your A records.
8. Save. Check propagation with `dig happyhikerproperties.com +short` (should list the four 185.199.x.153 IPs) or at dnschecker.org.
9. **Domain verification (step 9 above):** GitHub will give you a **TXT** record like `_github-pages-challenge-nacler-83` with a random value. Add it in GoDaddy as Type **TXT**, Name `_github-pages-challenge-nacler-83` (GoDaddy appends the domain automatically), Value = the string GitHub shows. Then click **Verify** in GitHub.

## Repo visibility note

GitHub Pages is free for **public** repositories. A private repo needs a paid plan (GitHub Pro or higher). This site has nothing sensitive, so a public repo is fine.

## Future phases (for reference)

- **Phase 2:** Property page with a photo gallery, description, amenities, location/map, and nearby hikes
- **Phase 3:** Contact or inquiry form (e.g. Formspree or Netlify Forms; no server needed) and an email signup for launch news
- **Phase 4:** Availability calendar or link out to Airbnb/VRBO, reviews/testimonials, and privacy-friendly analytics
- If the site grows to several pages, think about moving to a simple static site generator (Jekyll, which GitHub Pages builds natively, or Astro/Eleventy)

## Decisions (confirmed)

1. **Tagline:** "Your cabin in the woods awaits."
2. **Contact email:** none for now.
3. **Photo:** free-license stock photo (Unsplash or Pexels) for launch.
4. **Registrar:** GoDaddy (see step-by-step above).
5. **Region:** not mentioned for now.
