# QTFM Lab Website — Quantum Theory of Functional Materials, IISER Kolkata

**Project Type:** Academic Laboratory Website (Multiple Versions)  
**Client:** Quantum Theory of Functional Materials (QTFM) Lab, IISER Kolkata  
**PI:** Dr. Bheemalingam Chittari (Assistant Professor, Dept. of Physical Sciences)  
**Tech Stacks:** 
- Version 1: Angular 8 (bheem-app) — SPA with Firebase
- Version 2: PHP/MySQL — Traditional server-rendered with admin CMS
- Version 3: Simple redirect page → https://bheemalingam.com  
**Status:** Multiple iterations; live version redirects to external site  
**Developer:** Deepanshu Khairwal (The Setting Sun Web Solutions)  
**Last Updated:** February 2026  

---

## Overview

The QTFM Lab website has gone through multiple development iterations, reflecting different technical approaches:

1. **Angular 8 SPA** (`Bheema code/drive-download-20251214T103009Z-1-001/`) — Modern single-page application with Firebase hosting, but appears incomplete (missing app component source)
2. **PHP/MySQL CMS** (`My code/Website QTFM Lab/`) — Full-featured academic site with comprehensive database schema (13 tables), admin panel, and content management
3. **Redirect Page** (`My code/Website QTFM Lab/index.html`) — Current live version: 6-second animated redirect to `https://bheemalingam.com`
4. **Butterfly Animation** (`My code/butterfly_animate/`) — Creative WebGL/Canvas animation experiment

The SQL dump reveals a sophisticated academic website design with detailed PI profile, publications (33+ papers), research sections, team management, and full CMS capabilities.

---

## Version 1: Angular 8 Application (bheem-app)

### Project Structure
```
BC website/Bheema code/drive-download-20251214T103009Z-1-001/
├── src/
│   ├── index.html           # Entry point with Firebase SDK
│   ├── main.ts              # Bootstrap (enableProdMode, platformBrowserDynamic)
│   ├── polyfills.ts         # Zone.js, core-js
│   ├── styles.css           # Global styles (23KB - extensive)
│   ├── test.ts              # Karma test entry
│   ├── main-green.css       # Empty/placeholder
│   ├── assets/              # Static assets
│   └── environments/        # environment.ts, environment.prod.ts
├── public/                  # Built output (production)
│   ├── index.html           # Minimal (1.3KB) - references hashed bundles
│   ├── main-es2015.*.js     # ES2015 bundle (~500KB)
│   ├── main-es5.*.js        # ES5 bundle (~500KB)
│   ├── polyfills-*.js       # Polyfills (~37KB/127KB)
│   ├── styles.*.css         # Extracted styles (~205KB)
│   ├── scripts.*.js         # Vendor scripts (~147KB)
│   └── assets/              # Fonts, images
├── angular.json             # Angular CLI 8.3 config
├── package.json             # Angular 8.1.2, RxJS 6.4, Zone.js 0.9.1
├── tsconfig.*.json          # TypeScript 3.4.3 configs
├── tslint.json              # Codelyzer rules
├── karma.conf.js            # Jasmine/Karma testing
└── protractor.conf.js       # E2E testing
```

### Key Dependencies
```json
{
  "@angular/*": "~8.1.2",
  "angular-font-awesome": "^3.1.2",
  "bootstrap": "^4.4.1",
  "font-awesome": "^4.7.0",
  "jquery": "^3.4.1",
  "rxjs": "~6.4.0",
  "zone.js": "~0.9.1"
}
```

### Build Configuration
- **Output:** `dist/bheem-app`
- **Styles:** Bootstrap + FontAwesome + custom `styles.css`
- **Scripts:** jQuery + Bootstrap JS
- **Firebase:** Hosting via `/__/firebase/` reserved URLs (v7.11.0)
- **AOT:** Enabled for production
- **Budgets:** 2MB warning, 5MB error for initial bundle

### Global Styles (`src/styles.css` — 23KB)
Comprehensive CSS framework with:
- **Typography:** Montserrat, Open Sans, Raleway (Google Fonts)
- **Color Palette:** `#005490` (primary blue), `#7cc576` (accent green), `#222222` (dark)
- **Layout:** `.main-nav-outer`, `.main-section`, card grids)
- **Components:** Cards, service blocks, portfolio filters (Isotope), team circles, contact forms, footer
- **Animations:** CSS keyframe delays (`.delay-02s` through `.delay-12s`), hover rotate effects
- **Responsive:** Breakpoints at 767px, 768-1271px, 1271px+
- **Background:** Full-page `bgr.png` with overlay sections

### Firebase Integration
```html
<!-- In index.html -->
<script src="/__/firebase/7.11.0/firebase-app.js"></script>
<script src="/__/firebase/7.11.0/firebase-analytics.js"></script>
<script src="/__/firebase/init.js"></script>
```
- Firebase Hosting reserved URLs (`/__/firebase/`)
- Analytics enabled
- `init.js` contains project config (not in repo)

### Status: Incomplete
- `src/app/` directory exists but **empty** — no components, modules, or routing
- Likely a scaffolded project where app code was lost or not included in download
- Only compiled `public/` output represents a working build (from Sept 2021)

---

## Version 2: PHP/MySQL CMS (Full-Featured Academic Site)

### Database Schema (13 Tables)

| Table | Purpose | Key Fields |
|-------|---------|------------|
| `announcements` | Site-wide notices | title, content, type (info/warning/success/urgent), dates, display_order |
| `contact_messages` | Contact form submissions | name, email, subject, message, IP, admin reply fields |
| `footer_links` | Navigation links | label, url, display_order, is_editable |
| `gallery_items` | Photo gallery | title, caption, image_path, tags, display_order |
| `lab_news` | News/blog posts | title, content_html, image_path, published_date |
| `pi_academics` | PI education history | degree, institution, start/end year, description |
| `pi_honors` | PI awards/honors | title, description, year, display_order |
| `pi_invited_talks` | PI speaking engagements | title, event, institution, location, date |
| `pi_positions` | PI career positions | role, institution, start/end date, is_current |
| `pi_presentations` | PI conference presentations | title, event, location, date |
| `pi_profile` | PI main profile | name, designation, motivation, photo_path, cv_path |
| `publications` | Research publications | title, authors, journal, year, doi_url, pdf_path, is_featured |
| `research_sections` | Research area pages | title, intro_quote, content_html, media (image/gif/video) |
| `site_settings` | Global config | pi_name, pi_email, social URLs, lab address, maps embed |
| `teaching_materials` | Course materials | course_title, material_title, media_type (pdf/slides/video/link) |
| `team_members` | Lab team | name, role, category (PI/Postdoc/PhD/MSc/Alumni), bio, photo |
| `users` | Admin/auth users | email, password_hash, is_approved, email_verified |

### Key Data Highlights

#### PI Profile
- **Name:** Dr. Bheemalingam Chittari
- **Designation:** Associate Professor, Dept. of Physical Sciences, IISER-Kolkata
- **Email:** bheemalingam@iiserkol.ac.in
- **Motivation:** *"I am motivated to explore new areas of Nano Science of materials and Condensed matter."*

#### Publications (33 entries, 2011-2021)
Major journals: **Nature Physics, Science, Phys. Rev. B, Phys. Rev. Lett., J. Phys. Chem. C, J. Appl. Phys.**
Key topics:
- Graphene moiré superlattices (twisted bilayer/trilayer)
- Gate-tunable topological flat bands
- 2D magnetism (CrI₃, Cr₂Te₃, MPX₃ trichalcogenides)
- Fe-Pt magnetic nanoparticles
- High-pressure studies
- Ammonia borane oligomers

#### Research Sections (6)
1. **Magnetic Nano Particles** (order 1) — Fe-Pt alloys, core-shell segregation
2. **Graphene Moiré Superlattices** (order 2) — Twist-angle physics, flat bands
3. **Electronic, Bonding, Elastic & Optical Properties** (order 3) — DFT methodology
4. **High Pressure** (order 4) — Phase transitions, equation of state
5. **Oligomers** (order 5) — Molecular building blocks, charge segregation
6. **Tunable 2D Magnetism** (order 6) — MAX₃ trichalcogenides, strain/carrier tuning

#### PI Career Positions
- Assistant Professor, IISER-Kolkata (2021–present) **current**
- Research Professor, Univ. of Seoul (2018-2021)
- Visiting Postdoc, UC Berkeley (2018)
- Visiting Postdoc, UT Austin (2018)
- Postdoc, Univ. of Seoul (2016-2018)
- Postdoc, SKKU (2015-2016)
- Postdoc Fellow, Dr. Vijay Kumar Foundation (2013-2015)

#### Invited Talks (10+)
- IIT Guwahati, IIACS Kolkata, IISER Pune, IIT Bombay, TIFR Mumbai, IIT Guwahati, IIT Jodhpur (2019-2020)
- Virtual conferences: GRAPHENE2020, KPS Spring 2020, APS March Meeting

#### Honors & Awards
- Walter Kohn Award (ICCP10, 2017) — Best Paper
- Invited Reviewer: PRL, PRB, Nature Communications
- Hot Topic: ISSPIC-XVII (2014)
- Dr. K.V. Rao Research Award, 1st runner-up (2011)

---

### PHP Site Structure (Inferred from SQL + htdocs.zip)

```
My code/Website QTFM Lab/
├── index.html              # Redirect page (current live)
├── b11_41229538_dtb_qtfm_lab.sql  # Full DB schema + data
├── htdocs.zip              # PHP site files (953KB)
├── qtfm_lab.zip            # Full backup (70MB)
├── index.html              # Redirect page (identical)
├── Passwords Bheema.md     # Credentials (local only)
├── RECOVERY-CODES-contact.txt
└── noreply_OneAuth_BackupCodes_2026-02-13.txt
```

### Pages Implied by Footer Links & DB
| Page | Route | Content Source |
|------|-------|----------------|
| Home | `index.php` | `site_settings`, `announcements`, `research_sections` |
| Research | `research.php` | `research_sections` (6 areas) |
| Publications | `publications.php` | `publications` table (33 papers) |
| Team | `team.php` | `team_members` (categorized) |
| Teaching | `teaching.php` | `teaching_materials` |
| Gallery | `gallery.php` | `gallery_items` |
| About/Contact | `about.php#contact` | `site_settings`, `contact_messages` |
| Admin Login | `login.php` | `users` table |

### Admin Features (Inferred)
- CRUD for all content tables
- Announcement scheduling (start/end dates)
- Publication management with DOI/PDF
- Team member categorization (PI/Postdoc/PhD/MSc/Alumni)
- PI profile: academics, positions, honors, talks, presentations
- Contact message management with admin reply
- Site settings: social links, address, Google Maps
- Footer link management
- User approval system (`is_approved`, `email_verified`)

### Hosting History
- **Database Host:** `sql211.byethost11.com` (free MySQL hosting)
- **PHP Version:** 7.2.22
- **Server:** MariaDB 11.4.10
- **Export Date:** Feb 24, 2026

---

## Version 3: Redirect Page (Current Live)

### `My code/Website QTFM Lab/index.html`
**Purpose:** Temporary landing page redirecting to `https://bheemalingam.com`

### Design
- **Color Scheme:** Green primary (`#1f9851`), purple links (`#7d5fff`)
- **Animation:** 8 pulsating lines (top/bottom/left/right) with staggered delays
- **Card:** Centered white card with rounded corners, subtle shadow
- **Progress Bar:** 6-second linear fill animation (green)
- **Countdown:** JavaScript timer (6 → 0), then `window.location.href`

### Code Highlights
```css
/* Pulsating lines */
@keyframes pulsate {
  0% { opacity: 0.2; transform: scaleX(1); }
  50% { opacity: 0.5; transform: scaleX(1.05); }
  100% { opacity: 0.2; transform: scaleX(1); }
}
/* 8 lines with staggered animation-delay */

/* Progress bar */
.loader-fill {
  animation: fillBar 6s linear forwards;
}
@keyframes fillBar { from { width: 0%; } to { width: 100%; } }
```

```javascript
let timeLeft = 6;
setInterval(() => {
  timeLeft--;
  if (timeLeft <= 0) window.location.href = "https://bheemalingam.com";
}, 1000);
```

### Target URL
**https://bheemalingam.com** — Presumably the PI's personal/academic site or new lab website

---

## Version 4: Butterfly Animation (Creative Experiment)

### `My code/butterfly_animate/butterfly.html`
**Purpose:** Interactive WebGL/Canvas 2D animation of a butterfly surface

### Technical Implementation
- **Canvas 2D API** (not WebGL) — `canvas.getContext("2d")`
- **Implicit Surface Rendering** — Mathematical butterfly wing surface
- **3D Projection** — Custom orthographic projection with depth
- **Animation Loop:** `requestAnimationFrame` at ~60fps

### Mathematics
```javascript
// Wing boundary (parametric)
function insideWing(x, y) {
  const yLimit = Math.log(Math.abs(x) + 1);
  const xLimit = 2 + Math.sin(y);
  return y >= 0 && y <= yLimit && x >= 0 && x <= xLimit;
}

// Surface: z = ±flap * x² (flap = sin(t) oscillates)
const z = sign * flap * x * x;
```

### Rendering
- **Horizontal rulings:** y = 0 to 2.2, step 0.08
- **Vertical rulings:** x = 0 to 2.8, step 0.12
- **Stroke:** `#44ff44` (bright green), lineWidth 0.6
- **Axes:** Gray lines for X/Y/Z reference
- **Background:** Black (`body { background: black }`)

### Files
- `animate_butterfly.json` (2.8MB) — Likely exported animation data
- `animate_butterfly.lottie` (2MB) — Lottie/Bodymovin format
- `animate_butterfly.webm` (293KB) — Video export
- `butterfly.html` (2.8KB) — Interactive canvas version

---

## Comparative Analysis

| Aspect | Angular 8 | PHP/MySQL | Redirect Page | Butterfly |
|--------|-----------|-----------|---------------|-----------|
| **Architecture** | SPA (Client-side) | SSR (Server-side) | Static HTML | Canvas Animation |
| **CMS** | None (incomplete) | Full admin panel | None | None |
| **Database** | Firebase (planned) | MySQL/MariaDB | None | None |
| **Content Depth** | N/A | **Comprehensive** (33 pubs, 6 research areas, full PI profile) | Minimal | N/A |
| **Deployment** | Firebase Hosting | Byethost11 (free) | Static (any) | Static (any) |
| **Status** | **Incomplete** (no app code) | **Complete** (DB + inferred PHP) | **Live** (current) | **Experiment** |
| **Last Active** | Sept 2021 (build) | Feb 2026 (DB export) | Feb 2026 | Jan 2026 |

---

## Technical Debt & Issues

### Angular Version
- ❌ `src/app/` completely empty — no components, services, routing
- ❌ Firebase config only in `init.js` (not in repo)
- ❌ Angular 8 is EOL (Nov 2020) — needs migration to 17+
- ❌ Dependencies outdated (RxJS 6, Zone.js 0.9, TSLint deprecated)

### PHP Version
- ⚠️ PHP 7.2 EOL (Nov 2020) — security risk
- ⚠️ Free hosting (Byethost11) — unreliable for production
- ⚠️ No version control for PHP files (only SQL dump in repo)
- ⚠️ `htdocs.zip` not extracted in repo
- ✅ Database schema is well-designed and comprehensive

### Redirect Page
- ⚠️ Hardcoded 6-second delay — poor UX for slow connections
- ⚠️ No fallback if JS disabled
- ⚠️ Single point of failure (external domain)

---

## Recommendations

### Immediate
1. **Extract `htdocs.zip`** and add PHP source to version control
2. **Migrate from Byethost11** to proper hosting (VPS, Netlify Functions, Vercel, or IISER infrastructure)
3. **Upgrade PHP** to 8.2+ and MySQL to 8.0+
4. **Implement HTTPS** and security headers

### Medium-term
1. **Choose one stack** — Angular/React/Vue (modern SPA) OR PHP/Laravel (traditional) OR Static Site Generator (Astro/11ty)
2. **Migrate database** to PostgreSQL or keep MySQL with proper hosting
3. **Build proper admin UI** — the SQL schema is excellent, needs frontend
4. **Add authentication** — JWT or session-based with proper password hashing (bcrypt/argon2)

### Long-term
1. **Content strategy** — Keep publications updated via ORCID/Google Scholar API
2. **Performance** — Image optimization, CDN, caching
3. **Accessibility** — WCAG 2.1 AA compliance
4. **Analytics** — Plausible/Matomo (privacy-friendly) instead of Google Analytics

---

## Credits & Attribution

- **PI:** Dr. Bheemalingam Chittari (bheemalingam@iiserkol.ac.in)
- **Institution:** IISER Kolkata, Dept. of Physical Sciences
- **Developer:** Deepanshu Khairwal (The Setting Sun Web Solutions)
- **Research Focus:** Computational nanomaterials, graphene moirés, 2D magnetism, DFT
- **Key Collaborators:** Jeil Jung, Allan H. MacDonald, Feng Wang, Guorui Chen
- **Libraries:** Angular 8, Bootstrap 4, FontAwesome, jQuery, Firebase, Chart.js (implied)

---

*Document generated: July 2026*  
*Repository: https://github.com/dhzso/web-builder*  
*Primary Folder: `BC website/`*