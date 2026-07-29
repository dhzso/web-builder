# Pramanik Lab Website — Integrative Biology Laboratory, IISER Kolkata

**Project Type:** Academic Laboratory Website with CMS  
**Client:** Pramanik Lab (Integrative Biology Laboratory), IISER Kolkata  
**PI:** Dr. Subrata Pramanik (Assistant Professor, Dept. of Biological Sciences)  
**Tech Stack:** PHP 8.0+, SQLite 3, HTML5, CSS3, Vanilla JavaScript, Apache  
**Architecture:** Custom MVC-style with admin panel, dynamic content management  
**Status:** Production-ready (actively developed)  
**Developer:** Deepanshu Khairwal (The Setting Sun Web Solutions)  
**Last Updated:** July 2026  

---

## Overview

A modern, editorial-style academic laboratory website for the Pramanik Lab (Integrative Biology Laboratory) at IISER Kolkata. The lab uses planarians (Schmidtea mediterranea) as a model organism to study regeneration, stem cell biology, and neural repair. The site features a distinctive design with a regeneration process visualization, animated statistics, and a clean editorial aesthetic.

**Repository Folder:** `SP website/`  

---

## Architecture & Structure

### Directory Layout
```
SP website/
├── index.php                    # Homepage — hero, intro, regeneration stages, stats, partners
├── people.php                   # Team members (PI + researchers)
├── research.php                 # Research themes
├── publications.php             # Publications by year & type
├── facilities.php               # Facilities by workflow
├── gallery.php                  # Photo gallery by category
├── news.php                     # Announcements
├── contact.php                  # Contact form + info + Google Maps
├── pi-profile.php               # PI detailed profile
├── profile.php                  # Generic profile page
├── lab-feature.php              # Lab feature detail
├── features.php                 # All features listing
├── positions.php                # Open positions
├── .htaccess                    # Security, rewrites, HTTPS
├── admin/
│   ├── index.php               # Dashboard with content counts
│   ├── login.php               # Secure login with CSRF
│   ├── logout.php              # Session destruction
│   ├── change-password.php     # Password change
│   ├── records.php             # Generic CRUD list view
│   ├── edit.php                # Generic create/edit form
│   ├── delete.php              # Delete confirmation
│   ├── backup.php              # One-click database backup
│   └── includes/
│       ├── layout.php          # Admin header/footer
│       └── tables.php          # Table configuration
├── assets/
│   ├── css/
│   │   ├── site.css            # Main stylesheet (37KB) - editorial design
│   │   └── admin.css           # Admin panel styles
│   ├── js/
│   │   └── site.js             # Mobile nav, regen stage switcher, counters
│   ├── images/
│   │   ├── planaria-hero.png           # Hero microscopy image
│   │   ├── planaria.png                # Planarian organism photo
│   │   ├── planaria-regeneration-micrograph.png
│   │   ├── single-cell-atlas-editorial.png
│   │   ├── contact_page.png
│   │   ├── prl_lab logo.png
│   │   ├── lab_logo.png
│   │   ├── Iiser-Kolkata-Logo-Vector.png
│   │   ├── funding and collab/         # DST, ANRF, IISER logos
│   │   ├── Journo/                     # Journal cover images
│   │   └── regeneration/               # 4-stage regeneration images
│   └── uploads/                 # User-uploaded content
│       ├── gallery/
│       ├── people/
│       ├── positions/
│       ├── publications/
│       └── research/
├── includes/
│   ├── config.php              # Constants, helpers (url_for, asset_url, e())
│   ├── auth.php                # Authentication helpers
│   ├── database.php            # PDO connection + ALL query functions
│   └── layout.php              # render_header(), render_footer()
├── database/
│   ├── planaria_lab.sqlite     # SQLite database (69KB)
│   └── backups/                # Timestamped backups
└── storage/
    └── sessions/               # PHP session files
```

---

## Database Schema (SQLite)

The database (`planaria_lab.sqlite`) includes these tables:

| Table | Purpose |
|-------|---------|
| `announcements` | News/updates with category, dates, homepage display |
| `hero_slides` | Homepage hero slider |
| `lab_features` | Highlighted lab capabilities |
| `people` | Team members (PI, Current Members, Former Members) |
| `pi_profile` | PI detailed profile |
| `research_themes` | Research focus areas |
| `research_features` | Featured research projects |
| `publications` | Academic publications |
| `gallery_items` | Photo gallery by category |
| `facility_items` | Lab facilities by workflow |
| `open_positions` | Job openings |
| `contact_submissions` | Contact form entries |
| `admin_users` | Admin accounts |
| `news` | Lab news/posts |

### Key Seed Data (from `includes/database.php`)

#### Publications (7 entries, 2017-2024)
| Year | Title | Journal | Type |
|------|-------|---------|------|
| 2024 | Planarian single-cell atlas... | Frontiers in Cell and Developmental Biology | Journal |
| 2024 | Zebrafish in understanding... | The Journal of Gene Medicine | Journal |
| 2023 | In silico analysis decodes TTR... | Archives of Toxicology | Journal |
| 2022 | Surface charge engineering of Bacillus subtilis lipase A... | ACS Sustainable Chemistry & Engineering | Journal |
| 2019 | How To Engineer Ionic Liquids Resistant Enzymes... | ACS Sustainable Chemistry & Engineering | Journal |
| 2017 | Neurotrophin Signaling and Stem Cells... | Molecular Neurobiology | Journal |

#### Research Themes (Implied from homepage)
- Single-cell genomics & regeneration atlas
- Neoblast differentiation & RNAi
- Neural circuit reconstruction
- Computational morphometrics & graph learning

#### Facilities (4 workflow-based)
1. **Single-cell Capture Suite** — Droplet capture, library prep, QC
2. **Light-sheet Imaging Bay** — Long-duration wound response imaging
3. **Spatial Omics Studio** — Tissue registration, probe imaging
4. **Computational Connectomics Core** — Graph models, lineage inference

#### People
- **Dr. Subrata Pramanik** — PI, Assistant Professor
- **Current Research Members** — 2 (computational genomics, functional genetics)
- **Former Members** — 1 (Alumni placeholder)

#### News (3 entries)
- Atlas portal enters beta (2026-06-24)
- New project: neural reconnection maps (2026-05-12)
- Positions open for computational biologists (2026-04-04)

---

## Core PHP Functions

### Database Layer (`includes/database.php`)
```php
function db(): PDO                    // Singleton PDO connection to planaria_lab.sqlite
function rows(string $sql, array $params = []): array
function row(string $sql, array $params = []): ?array
function seed()                       // Initialize tables + sample data
```

### Data Access Functions (all in `database.php`)
```php
function research_themes(): array
function latest_news(int $limit = 6): array
function open_position(): ?array
function publications(): array
function people_groups(): array           // Grouped by category
function facilities_by_workflow(): array
function gallery_groups(): array
```

### Helpers (`includes/config.php`)
```php
const SITE_NAME = 'Pramanik Lab';
const SITE_TAGLINE = 'Integrative Biology Laboratory, IISER Kolkata';
const BASE_PATH = '';
const SITE_ROOT = __DIR__ . '/..';

function url_for(string $path = ''): string
function asset_url(string $path): string
function e(?string $value): string
function active_class(string $page, string $currentPage): string
```

### Layout (`includes/layout.php`)
```php
function render_header(string $title, string $currentPage = ''): void
function render_footer(): void
// Navigation: Home, Research, Publications, People, Facilities, Gallery, News, Contact, Positions
```

---

## Frontend Pages Detail

### 1. **Homepage (`index.php`)** — *Editorial Masterpiece*

#### Hero Section (`.editorial-hero`)
- Full-width microscopy image (`planaria-hero.png`)
- Overlay content: "Pramanik Lab" eyebrow, "A laboratory for the biology of rebuilding." headline
- Description: Planarian regeneration, neoblasts, stem-cell biology, brain regeneration
- CTAs: "What we study" → `research.php`, "Meet the lab" → `people.php`

#### Why Planaria (`.editorial-intro`)
- Editorial layout: text left, figure right
- Figure: `planaria.png` with caption citing "EEK Wisconsin"
- Copy: Neoblasts, stem-cell biology, regenerative medicine, gene-regulatory programmes

#### Regeneration Process (`.regeneration-process`) — *Signature Feature*
**4-Stage Interactive Visualization:**
| Stage | Kicker | Title | Image |
|-------|--------|-------|-------|
| 01 | Stage 01 / wound closure | The cut surface is sealed before new tissue is built. | `stage-1-wound-closure.png` |
| 02 | Stage 02 / neoblast response | Adult stem cells enter the repair programme. | `stage-2-neoblast-response.png` |
| 03 | Stage 03 / blastema and differentiation | A blastema forms and cell fates begin to separate. | `stage-3-blastema-differentiation.png` |
| 04 | Stage 04 / pattern restoration | Regeneration resolves into organized anatomy. | `stage-4-pattern-restoration.png` |

**Interaction:** Side panel with stage cards, bottom control buttons (01-04), JS-driven switching
**Disclaimer:** "Illustrative research figures, not raw experimental data."

#### Statistics (`.lab-stats-section`) — *Animated Counters*
| Stat | Value (Dynamic) | Label |
|------|-----------------|-------|
| Publications | `count(publications)` | Publications |
| Research Areas | `COUNT(research_themes)` | Research Areas |
| Team Members | `COUNT(people WHERE is_current=1)` | Team Members |
| Years Active | `date('Y') - min(publication year) + 1` | Years Active |

**JS:** `data-stats-counter` + IntersectionObserver triggers count-up animation

#### Partners (`.partners-section`)
- Horizontal logo scroller (CSS animation)
- Logos: DST GOI, ANRF, IISER Kolkata
- `data-partners-scroll` for infinite scroll

---

### 2. **People (`people.php`)**
- Groups: Principal Investigator → Current Members → Former Members
- Cards: Photo, name, role, bio snippet, profile link
- PI profile links to `pi-profile.php`

### 3. **Research (`research.php`)**
- Research themes from `research_themes()` query
- Featured research images with captions

### 4. **Publications (`publications.php`)**
- Grouped by year (descending)
- Fields: Authors, title, journal, year, DOI, impact factor, type
- Featured flag support

### 5. **Facilities (`facilities.php`)**
- Grouped by workflow (from `facilities_by_workflow()`)
- Card layout: title, workflow tag, description, image

### 6. **Gallery (`gallery.php`)**
- Grouped by category (from `gallery_groups()`)
- Lightbox-ready

### 7. **News (`news.php`)**
- From `latest_news()` — title, category, body, published date
- Category badges

### 8. **Contact (`contact.php`)**
- Form: Name, Email, Subject, Message
- Saves to `contact_submissions`
- Contact info from `site_settings` (if implemented)

### 9. **PI Profile (`pi-profile.php`)**
- Dedicated PI page with full bio, education, research interests, awards

---

## Admin Panel

### Authentication (`includes/auth.php` + `admin/login.php`)
- Session-based with timeout
- CSRF token protection
- Password hashing (needs verification — likely `password_hash()`)

### Dashboard (`admin/index.php`)
- Content counts per section
- Quick "Add new" links
- Backup button

### Generic CRUD (`admin/records.php`, `edit.php`, `delete.php`)
- Driven by `admin/includes/tables.php`
- Dynamic form generation
- Image upload with validation
- Field types: text, textarea, checkbox, image, email, url, number, select

### Backup (`admin/backup.php`)
- Copies `planaria_lab.sqlite` to `database/backups/planaria_lab_YYYY-MM-DD_HH-MM-SS.sqlite`

---

## CSS Design System (`assets/css/site.css` — 37KB)

### Design Philosophy: **Editorial / Scientific Journal Aesthetic**

### Color Palette
```css
--ink: #1a1a1a;           /* Near-black text */
--ink-muted: #4a4a4a;     /* Secondary text */
--paper: #fdfdfd;         /* Off-white background */
--paper-alt: #f5f5f5;     /* Alternate background */
--accent: #006d77;        /* Teal primary */
--accent-hover: #004d52;  /* Darker teal */
--accent-light: #e0f4f5;  /* Light teal bg */
--gold: #b8860b;          /* Gold accent */
--border: #e5e5e5;        /* Subtle borders */
--focus: #006d77;         /* Focus ring */
```

### Typography
- **Headings:** `IBM Plex Serif` (editorial serif) — `font-display: swap`
- **Body:** `IBM Plex Sans` (clean sans-serif)
- **Scale:** Fluid `clamp()` for responsive headings
- **Kickers:** `.eyebrow` — uppercase, letter-spaced, gold color

### Layout System
- **Container:** `max-width: 1120px`, centered
- **Grid:** CSS Grid + Flexbox
- **Spacing:** Consistent rhythm (--space-1 through --space-9)
- **Breakpoints:** 480, 768, 1024, 1200px

### Signature Components

#### Hero (`.editorial-hero`)
```css
min-height: 100vh;
display: grid;
grid-template-rows: 1fr minmax(auto, 60vh);
background: var(--paper);
```
- Full-bleed image with content overlay
- Gradient fade at bottom

#### Regeneration Stage Switcher (`.regen-stage-layout`)
- Side-by-side: media panel (figures) + content panel (cards + controls)
- CSS-only active state switching via `data-regen-*` attributes
- JS enhances with button clicks

#### Stat Cards (`.stats-grid`)
- Large numeric display with `data-target` for JS animation
- Label below

#### Partner Scroller (`.partners-scroll`)
- CSS `animation: scroll 20s linear infinite`
- Pause on hover

#### Cards (`.research-card`, `.stat-card`, `.person-card`)
- Subtle border, hover elevation
- Consistent padding, typography

#### Forms
- Clean inputs with focus rings
- Validation states
- Button variants: `.button.primary`, `.button.secondary`

---

## JavaScript (`assets/js/site.js`)

### Features
1. **Mobile Navigation** — Hamburger toggle, body scroll lock, ARIA attributes
2. **Regeneration Stage Switcher** — Button clicks update `is-active` on figures/cards
3. **Stat Counter Animation** — IntersectionObserver, `requestAnimationFrame` count-up
4. **Partner Scroll Pause** — Hover pauses CSS animation
5. **Smooth Scroll** — Anchor links with header offset
6. **Announcement Rotator** — Auto-advance (if implemented)

### Regeneration Switcher Logic
```javascript
// Data attributes: data-regen-figure, data-regen-card, data-regen-goto
// Click handler updates .is-active on corresponding elements
// ARIA live region for screen readers
```

---

## Deployment & Server Requirements

### Requirements
- **PHP**: 8.0+ (tested on 8.2+)
- **Extensions**: `pdo_sqlite`, `gd`, `session`, `mbstring`
- **Web Server**: Apache (`.htaccess` required)
- **Write Permissions**:
  - `database/` (for `planaria_lab.sqlite`)
  - `storage/sessions/`
  - `assets/uploads/` and all subdirectories

### `.htaccess` Highlights
```apache
# Security headers
Header set X-Content-Type-Options nosniff
Header set X-Frame-Options SAMEORIGIN
Header set X-XSS-Protection "1; mode=block"
Header set Referrer-Policy strict-origin-when-cross-origin
Header set Content-Security-Policy "default-src 'self'; ..."

# Protect sensitive directories
<DirectoryMatch "^(includes|database|storage|admin/includes)">
    Require all denied
</DirectoryMatch>

# Clean URLs
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php [QSA,L]

# HTTPS redirect (enable for production)
# RewriteCond %{HTTPS} off
# RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

### Installation
1. Extract files to web root
2. Set permissions (755 dirs, 644 files, 666 database/planaria_lab.sqlite)
3. Visit homepage — `seed()` auto-runs on first DB connection
4. Access `/admin/login.php` — set up admin user
5. Configure HTTPS, uncomment redirect in `.htaccess`

---

## Unique Features & Innovations

### 1. **Regeneration Process Visualization**
The 4-stage interactive regeneration diagram is the site's signature feature — combining scientific accuracy with editorial design. Each stage has:
- Custom microscopy-style illustration
- Narrative description
- Smooth JS-driven transitions
- Accessibility disclaimer

### 2. **Editorial Design Language**
Unlike typical academic sites (Bootstrap-heavy, generic), this uses:
- Serif headlines (`IBM Plex Serif`)
- Generous whitespace
- Asymmetric layouts (text + figure)
- Kickers/eyebrows for hierarchy
- Gold accent for emphasis

### 3. **Animated Statistics**
Counters animate on scroll via IntersectionObserver — performs well, accessible.

### 4. **Workflow-Based Facilities**
Facilities grouped by scientific workflow (Capture → Imaging → Spatial Omics → Computational) rather than equipment type.

### 5. **Journal Cover Gallery** (`.Journo/` folder)
Actual journal covers from publications — adds credibility and visual interest.

### 6. **Funding Partners Scroller**
Animated logo strip for DST, ANRF, IISER Kolkata — professional touch.

---

## Content Highlights

### Research Focus
> "We use planarians to understand how living tissues remember their shape, replace missing parts and reconstruct the nervous system after injury."

### Key Scientific Themes
- **Neoblast biology** — Adult pluripotent stem cells
- **Single-cell atlases** — Regeneration time courses
- **Neural regeneration** — Brain/circuit reconstruction
- **Computational biology** — Graph models, lineage inference
- **Spatial transcriptomics** — Patterning mechanisms

### PI Profile: Dr. Subrata Pramanik
- Assistant Professor, Dept. of Biological Sciences, IISER Kolkata
- Research: Neurobiology, regenerative medicine, molecular engineering, drug development
- Email: subrata.pramanik@iiserkol.ac.in
- Publications span: Planarian regeneration, zebrafish models, PFAS toxicology, enzyme engineering, neurotrophin signaling

---

## Maintenance & Operations

### Daily
- Monitor error logs
- Check contact submissions

### Weekly
- Review content
- Verify backups exist

### Monthly
- Clean old backups
- Test contact form
- Review security

### Quarterly
- PHP/OS updates
- Content audit
- Backup restore test

---

## Technical Debt & Issues

1. **All query functions in `database.php`** — Violates separation of concerns; should be in `data.php`
2. **Seed data hardcoded in `database.php`** — Mixes schema + data + logic
3. **No `data.php` file exists** — Unlike AM website pattern
4. **Admin `tables.php` not visible** — Need to verify CRUD configuration
5. **Password hashing unconfirmed** — Check `auth.php`
6. **No pagination** — Admin lists load all records
7. **Image optimization** — No auto-resize/compression
8. **Email via PHP `mail()`** — Configure SMTP for production

---

## Comparison with AM Website (HPI Lab)

| Aspect | AM Website (HPI Lab) | SP Website (Pramanik Lab) |
|--------|---------------------|---------------------------|
| **Design** | Traditional academic | **Editorial/Journal-style** |
| **Signature Feature** | Hero slider + stats | **4-stage regeneration viz** |
| **Code Organization** | Separated `data.php` | All in `database.php` |
| **Typography** | System sans-serif | **IBM Plex Serif + Sans** |
| **Color** | Navy + Gold | **Teal + Gold** |
| **Facilities** | Simple grid | **Workflow-grouped** |
| **Partner Logos** | Static | **Animated scroller** |
| **JS Complexity** | Moderate | **Higher (stage switcher, counters)** |
| **DB** | SQLite (13 tables) | SQLite (13 tables, similar) |

---

## Credits & Attribution

- **PI:** Dr. Subrata Pramanik (subrata.pramanik@iiserkol.ac.in)
- **Institution:** IISER Kolkata, Dept. of Biological Sciences
- **Developer:** Deepanshu Khairwal (The Setting Sun Web Solutions)
- **Model Organism:** *Schmidtea mediterranea* (planarian)
- **Imagery:** Microscopy-style illustrations, journal covers, lab photos
- **Fonts:** IBM Plex Serif, IBM Plex Sans (Google Fonts, SIL OFL)
- **Libraries:** PHP PDO, SQLite, vanilla JS (no heavy frameworks)

---

*Document generated: July 2026*  
*Repository: https://github.com/dhzso/web-builder*  
*Project Folder: `SP website/`*