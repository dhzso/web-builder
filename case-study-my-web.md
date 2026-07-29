# Case Study: Deepanshu Khairwal — Personal Portfolio & The Setting Sun Web Solutions

**Project Type:** Personal Portfolio + Freelance Agency Landing Page  
**Role:** Solo Designer & Developer  
**Timeline:** 2024–2026 (actively maintained)  
**Live URL:** https://khairwal-deepanshu.web.app/  
**Tech Stack:** Static HTML/CSS/JS, Firebase Hosting, 15+ vendor libraries  

---

## What Works Well

### 1. **Rich Content Depth**
- **5 long-form blog posts** covering IISER life, Meghalaya travel, iGEM 2025, France trip, agency launch — genuine storytelling with original photography
- **4 detailed project case studies** spanning Python/MySQL, behavioural ecology research, C systems programming, and iGEM synthetic biology leadership
- **60+ curated photography images** with EXIF metadata CSV — demonstrates both technical skill and artistic eye

### 2. **Strong Visual Identity**
- Custom dark navbar with orange accent (`#F96D00`) — memorable, consistent
- Hero slider with parallax backgrounds and animated text (AOS + Animate.css)
- Photography gallery with Magnific Popup lightbox — smooth UX for image-heavy content
- Theme toggle (dark/light) persisted in localStorage — nice touch for a dev portfolio

### 3. **SEO & Social Foundations**
- Complete Open Graph + Twitter Card meta tags on every page
- Semantic HTML5 with ARIA labels throughout
- Preloaded Google Fonts (Poppins) with `font-display: swap`
- Structured data ready for rich snippets

### 4. **The Setting Sun Sub-Project**
- Clean single-page agency landing with animated background lines
- 6-second auto-redirect to `bheemalingam.com` with progress bar — clever interim solution
- Business registration docs organized in `/gst/` — professional delivery artifacts

---

## Technical Specifications

| Aspect | Detail |
|--------|--------|
| **Pages** | 12 HTML files (index, photography, 5 blogs, 4 projects, 404) |
| **CSS** | Modular source (base/components/layout/pages) + 256KB compiled `style.css` + 15 vendor stylesheets |
| **JS** | 14 vendor libs (jQuery, Bootstrap, Owl Carousel, AOS, Stellar, Waypoints, Magnific Popup, etc.) + 4 custom scripts |
| **Images** | ~2 GB total (JPG/PNG/WebP), lazy-loaded, organized in dated folders |
| **Hosting** | Firebase Hosting (global CDN, auto-SSL, custom domain) |
| **Forms** | Web3Forms serverless endpoint (no backend needed) |
| **Performance** | No build step — pure static files; all scripts `defer`/`async` |

---

## Improvements Needed

### High Priority
| Issue | Recommendation |
|-------|----------------|
| **No build system** | Migrate to Astro/11ty/Vite — enable minification, critical CSS extraction, image optimization (WebP/AVIF), cache-busting hashes |
| **Vendor bloat** | 400KB+ of unused CSS/JS (Bootstrap 4, 3 icon fonts, animate.css, etc.) — purge with PurgeCSS or switch to utility-first (Tailwind) |
| **Image optimization** | 2 GB raw images — add responsive `srcset`, WebP conversion, lazy-loading via `<picture>`, CDN transformation (Cloudinary/Imgix) |
| **Blog management** | Manual HTML per post — move to Markdown/MDX with frontmatter; auto-generate listing, RSS, sitemap |
| **Contact form** | Web3Forms exposes access key in HTML — use Netlify Forms, Formspree, or Firebase Functions for serverless handling |

### Medium Priority
- **Accessibility audit**: Color contrast on orange accent, focus states, heading hierarchy, alt text consistency
- **Analytics**: Add Plausible/Umami (privacy-friendly) — currently none
- **Caching headers**: Configure Firebase `headers` for long-term asset caching
- **404 page**: Exists but minimal — add search, site map, friendly copy

### Low Priority / Nice-to-Have
- Dark mode: currently only toggles CSS variables — add `prefers-color-scheme` detection
- PWA: Service worker for offline viewing of blog/portfolio
- Search: Client-side search (Pagefind/Algolia) for blog + photography
- Multi-language: Hindi/Haryanvi version for personal touch

---

## Verdict

**Strong content, genuine personality, solid foundation.** The portfolio succeeds because it *shows* not just *tells* — real photos, real projects, real writing. Technical debt is typical for a self-taught dev's first major site: vendor-heavy, no build pipeline, manual content management. A migration to a modern SSG (Astro recommended for content-heavy + island interactivity) would cut bundle size by ~80%, automate image optimization, and make blogging frictionless — while preserving the custom design system.

**The Setting Sun landing page** is a smart MVP: validates demand before building full agency site. The redirect pattern works for "coming soon" but should be replaced with a proper site once client work stabilizes.