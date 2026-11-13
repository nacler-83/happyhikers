# Happy Hiker Properties

Static "coming soon" page for https://happyhikerproperties.com, hosted on GitHub Pages. Plain HTML and CSS, no build step.

## Preview locally

```
python3 -m http.server
```

Then open http://localhost:8000.

## Swap the hero photo

Replace `assets/cabin-hero.jpg` (about 1920px wide, under ~400 KB) and `assets/og-image.jpg` (1200x630, used for social previews). Update the alt text in `index.html` and `404.html`, and the photo credit in the footer. If you add a WebP version, wrap the image in a `<picture>` with a `<source type="image/webp">`.

## Deploy

See `plan.md` for the GitHub Pages and GoDaddy DNS setup.
