The markup, styles, animations, music, and RSVP form live in `index.html`; editable invitation copy lives in `content.json`. No build step, no backend, no dependencies. Open it in a browser and it works.

# Yonan & Helana — Wedding Invitation

A bilingual (Arabic + English, RTL primary) wedding invitation web page for **Yonan & Helana · 1 October 2026**.

Everything — markup, styles, animations, music, RSVP form — lives in `index.html`. No build step, no backend, no dependencies. Open it in a browser and it works.

---

## What's inside

| Section            | Notes                                                                                          |
| ------------------ | ---------------------------------------------------------------------------------------------- |
| **Loading splash** | Wax-seal monogram with rotating ring; shows for 2.5–4s                                         |
| **Envelope cover** | Tap the envelope to open the invitation; wax-seal animation + photo lift                       |
| **Hero**           | Couple names with Jeremiah 32:39 verse in Arabic                                               |
| **Welcome**        | Optional per-guest greeting via `?to=` URL param                                               |
| **Save the Date**  | Date block + smart "Add to Calendar" (Google on desktop/Android, .ics on iOS) + WhatsApp share |
| **Countdown**      | Live ticking countdown to 2026-10-01 18:00 Cairo, paused when tab hidden                       |
| **Event cards**    | Church (5:30 PM) + Reception (7:00 PM) with map links                                          |
| **Photo gallery**  | Swipeable deck of `newImage1`–`newImage4`, keyboard arrows supported                           |
| **Timeline**       | 4-step wedding-day schedule                                                                    |
| **Good to Know**   | Dress code · Parking · Family · Gifts (editable defaults)                                      |
| **RSVP form**      | Confirms attendance with confetti; **frontend-only** (no backend)                              |
| **Music**          | Wagner's _Bridal Chorus_ synthesized via WebAudio + hall reverb; toggle bottom-left            |
| **Ambient**        | 36 falling petal/star glyphs across the viewport                                               |

---

## File structure

```
/
├── index.html               # the entire site
├── content.json             # all editable copy — names, date, times, locations, RSVP/calendar text
├── vercel.json               # security + cache headers
├── favicon.svg               # gold-coin monogram
├── og-card.{jpg,webp,avif}  # 1200×630 social-share previews (kept at root: referenced by absolute og:image URL)
├── images/                  # four invitation photos
│   ├── newImage1.jpeg
│   ├── newImage2.jpeg
│   ├── newImage3.jpeg
│   └── newImage4.jpeg
└── README.md
```

---

## Common customisations

### Edit the wedding copy (names, date, times, locations, RSVP/calendar text)

Edit `content.json` and reload the page (or redeploy to Vercel) — `index.html` reads it at load time via a `data-content` binding, so no HTML edits are needed for text changes. See the `_comment` at the top of `content.json` for details.

The remaining customisations below are single-line edits in `index.html` itself.

### Change the envelope cover photo

Update the `src` and `srcset` values for the cover photo, gallery photos, and preload link in `index.html`.

### Tune photo framing inside the envelope

On `.env-photo`:

```css
--env-photo-zoom: 1; /* 1 = natural fit; >1 crops in */
--env-photo-y: 50%; /* 0% = top, 50% = middle, 100% = bottom */
```

### Personalise per recipient

Append `?to=NAME` to the URL:

```
…/index.html?to=Yara
```

The welcome card shows _"أهلاً يا Yara ✦"_ and the RSVP name field auto-fills. Sanitised + capped at 40 chars.

### Update "Good to Know" copy

Edit the `info.items` array in `content.json` — four entries (Dress Code · Parking · Family · Gifts). Defaults are placeholders; edit the Arabic + English copy in place.

### Adjust the loading splash duration

Search for `SPLASH — wax-seal intro` in the `<script>` block:

```js
const MIN_MS = 2500; // minimum show time (ms)
const MAX_MS = 4000; // hard cap (ms)
```

### Brand palette

At the top of the `<style>` block under `DESIGN TOKENS`:

```css
--gold: #c9a35b;
--gold-deep: #9d7a2f;
--wine: #6b1f2a;
--paper: #fbf6ec;
```

---

## Deployment

It's a static page — host it anywhere:

- **GitHub Pages**: push to `main`, enable Pages → root → `/`.
- **Netlify / Vercel / Cloudflare Pages**: drag-and-drop the folder.
- **Direct file**: works from `file://` for local previews (note: `content.json` won't load over `file://` due to CORS — the page falls back to the hardcoded HTML defaults in that case).

On Vercel, `vercel.json` applies a strict CSP, HSTS, `Permissions-Policy`, and a 1-year `immutable` cache for `.avif/.webp/.jpg/.png/.svg/.woff2`. Other hosts will need an equivalent config to get the same security and caching behavior.

The site is currently configured for `https://yonan-helana.vercel.app/` (see the `og:` meta tags and the `img-src` in the CSP). Update those URLs if you move to a new domain.

### Regenerating image variants

The current invitation uses the four checked-in JPEG photos in `images/` directly.

---

## Accessibility

- WCAG AA contrast on body text and interactive states
- `:focus-visible` rings on every interactive element
- `aria-live` announcements for RSVP confirmation
- `aria-hidden` on the collapsed invitation until the envelope is tapped
- Keyboard navigation for the photo deck (← / →)
- `prefers-reduced-motion` disables ambient particles and decorative animations
- English content wrapped in `<span lang="en">` so Arabic screen readers pronounce names correctly
- `dir="ltr"` on Latin-only blocks (cover names, splash monogram) inside the RTL document

---

## Browser support

Tested on modern Chrome, Safari, Firefox (desktop + iOS + Android). WebAudio music gracefully no-ops on older browsers.

---

## Credits

- Fonts: Playfair Display · Marck Script · Tajawal · Cormorant Garamond (Google Fonts)
- Verse: Jeremiah 32:39 (Arabic translation)
- Music: _Bridal Chorus_ (Wagner, _Lohengrin_) — synthesized via WebAudio
- Photography: Couple's own

Made with love for **Yonan & Helana · 01 · 10 · 2026** 🤍
