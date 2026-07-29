# Case Study: QTFM Lab Website — Quantum Theory of Functional Materials, IISER Kolkata

**Project Type:** Academic Lab Website — Multiple Iterations (Angular SPA, PHP/MySQL CMS, Redirect, Creative Demo)  
**Client:** Dr. Bheemalingam Chittari, QTFM Lab, IISER Kolkata  
**Role:** Full-stack Developer (Deepanshu Khairwal / The Setting Sun Web Solutions)  
**Timeline:** 2021–2026 (4 distinct versions)  
**Live URL:** Redirects to https://bheemalingam.com  

---

## What Works Well

### 1. **PHP/MySQL Version — Comprehensive Academic Schema**
The SQL dump (`b11_41229538_dtb_qtfm_lab.sql`) reveals a **thoroughly designed 16-table schema** covering every academic need:
- **PI Profile Depth**: Academics (4 degrees), Positions (8 roles across 3 countries), Honors (10 awards), Invited Talks (10), Presentations (11) — far beyond typical "bio + photo"
- **Publications**: 33 papers (2011–2024) in *Nature Physics, Science, Phys. Rev. B, J. Phys. Chem. C, APS March Meetings* — real high-impact data
- **Research Sections**: 6 areas with intro quotes, rich HTML content, media support (image/GIF/video), display ordering
- **Team Management**: Category enum (PI/Postdoc/PhD/MSc/Alumni), featured flag, profile links
- **Teaching Materials**: Media-type enum (PDF/slides/video/link) — ready for course pages
- **Site Settings**: Social links, lab address, Google Maps embed, PI name/email — single source of truth
- **User Auth**: Email verification, approval workflow, password reset tokens

### 2. **Research Content Quality**
The PI's actual publication record is exceptional — gate-tunable flat bands in graphene moirés, 2D magnetism (CrI₃, Cr₂Te₃, MPX₃), Fe-Pt nanoparticles, high-pressure DFT. The schema captures this with proper fields (DOI, journal, year, featured flag, display order). This isn't placeholder data — it's a real professor's CV structured for the web.

### 3. **Angular 8 Version — Modern Foundation (If Completed)**
- Angular CLI 8.3, RxJS 6.4, Zone.js 0.9, TypeScript 3.4
- Firebase Hosting config ready (`/__/firebase/` reserved URLs, Analytics)
- Production build output exists in `public/` (hashed bundles, extracted CSS, code-split polyfills)
- Bootstrap 4 + FontAwesome + jQuery via `angular.json` scripts/styles arrays
- AOT + build optimizer + budgets (2MB warning / 5MB error) configured

### 4. **Creative Technical Exploration**
- **Butterfly Animation**: Canvas 2D implicit surface rendering with 3D projection, parametric wing boundary, animated `z = ±sin(t)·x²` — shows graphics/math capability
- **Redirect Page**: 6-second animated countdown with 8 pulsating lines + progress bar — polished interim UX

---

## Technical Specifications by Version

| Version | Stack | Status | Key Files |
|---------|-------|--------|-----------|
| **v1 Angular** | Angular 8, Firebase, Bootstrap 4 | **Incomplete** — `src/app/` empty, no components/routes | `package.json`, `angular.json`, `public/` (built Sept 2021) |
| **v2 PHP/MySQL** | PHP 7.2, MariaDB 11.4, Byethost11 | **Complete schema + data** — PHP files in `htdocs.zip` (not extracted) | `b11_41229538_dtb_qtfm_lab.sql` (54KB), `htdocs.zip` (953KB) |
| **v3 Redirect** | Static HTML/CSS/JS | **Live** — redirects to `bheemalingam.com` | `My code/Website QTFM Lab/index.html` |
| **v4 Butterfly** | Canvas 2D, vanilla JS | **Demo** — standalone animation | `butterfly_animate/butterfly.html` + exports (JSON/Lottie/WebM) |

---

## Improvements Needed

### Critical (Blocking Production Use)
| Version | Issue | Fix |
|---------|-------|-----|
| **All** | **No single deployed version** | Pick **one stack** and deploy it. Current live site is a redirect — not a lab website |
| **v2 PHP** | **PHP 7.2 EOL (Nov 2020)**, free hosting (Byethost11) | Migrate to PHP 8.2+ on VPS/Render/Railway/IISER infra; extract `htdocs.zip` to repo |
| **v1 Angular** | **Angular 8 EOL (Nov 2020)**, `src/app/` missing | If choosing Angular: migrate to 17+ (standalone components, signals, esbuild), rebuild app from schema |
| **v2 DB** | **No password hashing visible** in SQL — `users.password` likely plaintext | Implement `password_hash()` / `password_verify()`; add bcrypt/argon2 migration |

### High Priority (Whichever Stack Chosen)
| Area | Recommendation |
|------|----------------|
| **Unify content source** | Schema is excellent — use it as single source of truth. Build admin UI (Filament/Laravel, or custom) to manage it |
| **Publication sync** | 33 papers manual entry — add ORCID/Google Scholar/DBLP API import button |
| **Image pipeline** | No optimization — add upload resize (max 1920px), WebP conversion, CDN (Cloudinary/Imgix) |
| **Search** | SQLite FTS5 or Meilisearch for publications/people/talks |
| **Analytics** | Plausible/Matomo (not GA) — privacy-friendly for academic site |
| **Accessibility** | WCAG 2.1 AA: heading order, contrast, focus states, alt text, skip links |

### Medium Priority
- **Multi-language**: English + Hindi (IISER Kolkata serves diverse audience)
- **Versioned content**: Revisions for publications/talks (corrections, updates)
- **API layer**: Headless CMS → Astro/Next.js frontend for static generation + ISR
- **Teaching page**: Build out `teaching_materials` table into course pages with embedded PDFs/video
- **Newsletter**: Mailchimp/Buttondown integration for lab updates

### If Rebuilding from Scratch (Recommended Stack)
| Layer | Choice | Why |
|-------|--------|-----|
| **CMS/Backend** | **Laravel + Filament** | Admin panel auto-generated from models; schema maps 1:1; roles/permissions built-in |
| **Database** | **PostgreSQL** | Concurrent writes, JSONB for flexible fields, FTS, better than SQLite for multi-admin |
| **Frontend** | **Astro + Alpine.js** | Static-first, island interactivity, zero-JS default, content collections for publications/talks |
| **Styling** | **Tailwind CSS** | Design system in config, purge unused, dark mode trivial |
| **Images** | **Astro Assets + Cloudinary** | Auto WebP/AVIF, responsive `srcset`, blur placeholders |
| **Search** | **Pagefind** (static) or **Meilisearch** (dynamic) | Client-side or server-side, no Algolia cost |
| **Deploy** | **Vercel/Netlify/Cloudflare Pages** | Git push → global CDN, preview deploys, edge functions |
| **Auth** | **Laravel Sanctum + Filament** | Admin auth separate from public; API tokens for headless |

---

## Verdict

**The schema is the asset.** The 16-table MySQL design with 33 real publications, deep PI profile, and academic workflow awareness is better than 90% of lab websites *in production*. But it's trapped in:
- An incomplete Angular app
- A PHP 7.2 codebase on free hosting (not in repo)
- A redirect page
- A canvas demo

**The fix is architectural, not cosmetic:** Extract `htdocs.zip`, pick **one stack** (Laravel + Astro recommended), build the admin once from the schema, deploy. The content is ready — the plumbing isn't.

**Best insight from this project:** The SQL dump *is* the requirements document. Every table = a page/component. Every column = a field. Every index = a query pattern. Next time, start with the schema, generate the admin (Filament), build the frontend (Astro) — skip the 3 failed iterations.