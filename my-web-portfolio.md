# Deepanshu Khairwal — Personal Portfolio Website

**Project Type:** Personal Portfolio & Blog  
**Tech Stack:** HTML5, CSS3 (Custom + Libraries), Vanilla JavaScript, jQuery, Bootstrap 4  
**Hosting:** Firebase Hosting (khairwal-deepanshu.web.app)  
**Status:** Live & Actively Maintained  
**Last Updated:** June 2026  

---

## Overview

A comprehensive personal portfolio website showcasing the academic journey, research projects, photography, and blog of Deepanshu Khairwal — a Life Sciences student at IISER Kolkata with minors in Chemistry and Computer Science electives. The site serves as a digital CV, project showcase, photography portfolio, and personal blog.

**Live URL:** https://khairwal-deepanshu.web.app/

---

## Architecture & Structure

### Directory Layout
```
#My web/
├── My portfolio/                 # Main portfolio website
│   ├── index.html               # Homepage (hero, about, projects, blog, contact)
│   ├── photography.html         # Photography gallery (60+ images)
│   ├── blog-post1.html to blog-post5.html  # Individual blog posts
│   ├── project1.html to project4.html      # Project detail pages
│   ├── 404.html                 # Custom 404 page
│   ├── css/
│   │   ├── base/                # Base styles (reset, variables)
│   │   ├── components/          # Reusable components
│   │   ├── layout/              # Grid, header, footer
│   │   ├── pages/               # Page-specific styles
│   │   ├── animate.css          # Animate.css library
│   │   ├── aos.css              # AOS scroll animations
│   │   ├── style.css            # Main compiled stylesheet (256KB)
│   │   └── [15+ vendor libraries]
│   ├── js/
│   │   ├── main.js              # Custom scripts
│   │   ├── switch.js            # Dark/light theme toggle
│   │   ├── timeline.js          # Timeline animations
│   │   ├── skill_counter.js     # Animated skill counters
│   │   └── [12+ vendor libraries: jQuery, Bootstrap, Owl Carousel, AOS, etc.]
│   └── image/
│       ├── Home/                # Hero, profile, logo images
│       ├── Photography/         # 60+ photography images + metadata CSV
│       ├── blog1-5/             # Blog post images
│       └── proj1-4/             # Project screenshots & photos
│
├── The Setting Sun/             # Web solutions landing page
│   ├── index.html               # Single-page landing for "The Setting Sun Web Solutions"
│   ├── design.svg               # Brand design asset
│   ├── hpi-lab.png              # Portfolio piece: HPI Lab website
│   ├── qtfm-lab.png             # Portfolio piece: QTFM Lab website
│   ├── portfolio.png            # Portfolio showcase
│   └── gst/                     # Business registration documents
│
└── SITE_REPRODUCTION.md         # Comprehensive reproduction prompt for lab websites
```

---

## Key Features

### 1. **Multi-Page Portfolio Structure**
- **Home** (`index.html`): Hero slider, About, Projects grid, Blog preview, Contact form
- **Projects** (4 detail pages): Student Record Repository (Python/MySQL), Dog Cognition Research (Behavioural Ecology Lab), Flight Reservation System (C), iGEM 2025 IISER Kolkata (Synthetic Biology Wet Lab Lead)
- **Blog** (5 posts): Life at IISER Kolkata, Meghalaya Adventure, iGEM 2025 & SynBio Conference, France Trip, The Setting Sun Web Solutions launch
- **Photography** (`photography.html`): Full-screen gallery with 60+ curated images, metadata CSV
- **Contact**: Web3Forms-powered contact form + direct email/LinkedIn links

### 2. **Design System**
- **Typography:** Poppins (Google Fonts, preloaded)
- **Color Scheme:** Dark navbar (`bg-dark`), orange accent (`#F96D00`), white/light backgrounds
- **CSS Architecture:** Modular (base/components/layout/pages) + massive compiled `style.css` (256KB)
- **Animations:** AOS (Animate On Scroll), Animate.css, custom shake/timeline/skill-counter effects
- **Theme Toggle:** Dark/Light mode via `switch.js` + CSS custom properties

### 3. **Third-Party Libraries (Vendor)**
| Library | Purpose |
|---------|---------|
| Bootstrap 4.3 | Grid, utilities, components |
| jQuery 3.4.1 + migrate | DOM manipulation, plugins |
| Owl Carousel 2 | Hero slider, image carousels |
| AOS | Scroll-triggered animations |
| Magnific Popup | Image lightbox (photography) |
| Stellar.js | Parallax scrolling |
| Waypoints | Scroll-triggered counters |
| IonIcons, FontAwesome, IcoMoon, Flaticon | Icon systems |

### 4. **Contact Form**
- **Backend:** Web3Forms (serverless form endpoint)
- **Access Key:** `8ebb97ad-ffff-48e3-9f7f-f3dcd781f518`
- **Fields:** Name, Email, Subject, Message
- **Validation:** HTML5 required attributes

### 5. **SEO & Social**
- Full Open Graph tags (title, description, image, URL, site_name)
- Twitter Card (summary_large_image)
- Meta description, keywords, author
- Semantic HTML5 structure with ARIA labels
- Preloaded Google Fonts

### 6. **Photography Section Deep Dive**
- **60+ high-resolution images** organized in dated folders (2021-2026)
- **Metadata CSV** (`photo_metadata_pro.csv`) with EXIF data
- **Categories:** Landscapes, portraits, macro, astrophotography, events
- **Magnific Popup** integration for full-screen lightbox viewing
- **Lazy loading** on all images

### 7. **The Setting Sun Web Solutions** (Sub-project)
A separate single-page landing site (`#My web/The Setting Sun/`) showcasing the freelance web development service:
- Hero with animated background lines
- Portfolio cards linking to HPI Lab & QTFM Lab sites
- 6-second auto-redirect to `https://bheemalingam.com`
- Business registration documents in `/gst/` (invoices, trade name proof, electricity bill)

---

## Content Highlights

### Projects Showcased
1. **Students Record Repository** — Python/MySQL desktop app for school administration
2. **Dogs' Cognitive Abilities** — Behavioural Ecology Lab, IISER Kolkata (research assistant)
3. **Flight Reservation System** — C language console application
4. **iGEM 2025 — IISER Kolkata** — Synthetic Biology, Wet Lab Lead (team project)

### Blog Posts (Chronological)
| Date | Title | Focus |
|------|-------|-------|
| Oct 2024 | Life at IISER Kolkata | Campus life, sunrise/sunset photography |
| Mar 2025 | Meghalaya Adventures | Northeast India travel, waterfalls, caves |
| Jan 2026 | iGEM 2025 & SynBio Conference 8.0 | Synthetic biology competition experience |
| Jan 2026 | Trip to France | Science, cities, conversations |
| Jun 2026 | The Setting Sun Web Solutions | Freelance web dev service launch |

---

## Technical Implementation Details

### CSS Organization
```
css/
├── base/base.css              # CSS variables, reset, typography base
├── components/components.css  # Buttons, cards, forms, nav, overlays
├── layout/layout.css          # Grid, header, footer, sections
├── pages/
│   ├── blog.css              # Blog grid, post cards, meta
│   ├── photography.css       # Gallery grid, lightbox, masonry
│   └── projects.css          # Project cards, hover effects
├── style.css                 # Main compiled bundle (256KB)
└── vendor/                   # 15+ minified vendor stylesheets
```

### JavaScript Loading Strategy
```html
<!-- Preloaded/Async -->
<link rel="preload" href="fonts.googleapis.com/...Poppins" as="style">
<script src="jquery.min.js" defer></script>
<script src="modernizr.min.js" async></script>

<!-- Deferred vendor scripts -->
<script src="bootstrap.min.js" defer></script>
<script src="owl.carousel.min.js" defer></script>
<script src="aos.js" defer></script>
<!-- ... -->

<!-- App scripts (deferred) -->
<script src="main.js" defer></script>
<script src="switch.js" defer></script>
<script src="timeline.js" defer></script>
<script src="skill_counter.js" defer></script>
```

### Image Optimization
- **WebP/PNG/JPEG** mixed formats
- **Lazy loading** (`loading="lazy"`) on all images
- **Responsive backgrounds** via inline `style="background-image:url(...)"`
- **Preloaded hero images** in `<head>`

---

## Deployment & Hosting

### Firebase Hosting Configuration
- **Project ID:** `khairwal-deepanshu`
- **Hosting URL:** `https://khairwal-deepanshu.web.app`
- **Custom Domain:** Configured (DNS A/CNAME records)
- **SSL:** Automatic via Firebase
- **Caching:** Static assets cached via Firebase CDN

### Build Process
No build step required — pure static files. Deploy via:
```bash
firebase deploy --project khairwal-deepanshu
```

---

## Performance Metrics (Estimated)
- **Total CSS:** ~400KB (including vendors)
- **Total JS:** ~500KB (including vendors)
- **Images:** 2GB+ total (photography section dominant)
- **Critical Path:** Hero slider, navbar, above-fold content
- **Optimization Opportunities:** Code-split JS, WebP conversion, lazy-load all images, purge unused CSS

---

## Maintenance Notes

### Adding New Blog Posts
1. Create `blog-postN.html` from template
2. Add images to `image/blogN/`
3. Update blog grid in `index.html` (new card)
4. Add entry to blog index (if separate page exists)

### Adding Photography
1. Drop images in `image/Photography/`
2. Update `photo_metadata_pro.csv`
3. Gallery auto-populates via manual HTML (currently static)

### Theme Customization
- Edit CSS custom properties in `css/base/base.css`
- Primary color: `--primary: #F96D00`
- Dark mode toggle persists via `localStorage`

---

## Future Enhancement Ideas
- [ ] Migrate to static site generator (Astro/11ty) for blog management
- [ ] Convert photography to dynamic gallery (JSON-driven)
- [ ] Add search/filter for blog and photography
- [ ] Implement service worker for offline caching
- [ ] Migrate contact form to Netlify Forms or custom endpoint
- [ ] Add RSS feed for blog
- [ ] Optimize images (WebP, responsive srcset)
- [ ] Purge unused Bootstrap/components CSS

---

## Credits & Attribution
- **Design & Development:** Deepanshu Khairwal
- **Institution:** IISER Kolkata (Indian Institute of Science Education and Research)
- **Domain:** `khairwal-deepanshu.web.app` (Firebase)
- **Photography:** All images by Deepanshu Khairwal unless noted
- **Icons:** IonIcons, FontAwesome, IcoMoon, Flaticon (licensed)
- **Libraries:** All vendor libraries under respective MIT/BSD licenses

---

*Document generated: July 2026*  
*Repository: https://github.com/dhzso/web-builder*