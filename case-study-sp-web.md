# Case Study: Pramanik Lab Website — Integrative Biology Laboratory, IISER Kolkata

**Project Type:** Academic Lab Website with Custom PHP/SQLite CMS  
**Client:** Dr. Subrata Pramanik, Pramanik Lab, IISER Kolkata  
**Role:** Full-stack Developer (Deepanshu Khairwal / The Setting Sun Web Solutions)  
**Timeline:** ~June–July 2026 (active development)  
**Status:** Production-ready, SQLite-backed, editorial design  

---

## What Works Exceptionally Well

### 1. **Editorial-Grade Design System (`site.css` — 37KB)**
Not a generic Bootstrap template. Custom design language built for *scientific storytelling*:
- **Typography**: System font stack with `clamp()` fluid scaling, "eyebrow" kickers (`p.eyebrow`), generous line-height
- **Color Palette**: Deep navy (`#0f172a`), warm gold (`#c9a84c`), sage green (`#4a7c59`), off-white (`#fafafa`) — academic but not sterile
- **Layout Primitives**: `.section-heading`, `.card-grid`, `.stat-card`, `.regen-stage-layout` — reusable, semantic
- **Motion**: CSS-only counters (`data-stats-counter`), scroll-reveal via `IntersectionObserver`, regeneration stage switcher (CSS `:target` + JS fallback)
- **No Framework Bloat**: Zero Bootstrap, zero jQuery, zero Fancybox — vanilla CSS Grid/Flexbox + 2KB JS

### 2. **Regeneration Process Visualization — Signature Feature**
Homepage centerpiece: 4-stage planarian regeneration timeline
```
Stage 01: Wound Closure      →  microscopy-style illustration
Stage 02: Neoblast Response  →  stem cell activation imagery  
Stage 03: Blastema Formation →  differentiation visualization
Stage 04: Pattern Restoration →  whole-organism recovery
```
- **Desktop**: Fixed media column (sticky images) + scrolling text panels
- **Mobile**: Carousel with dot navigation
- **Data-driven**: Stages defined in `index.php` array → easy to reorder/edit
- **Credibility Note**: "Illustrative research figures, not raw experimental data" — ethical science communication

### 3. **SQLite + Single-File CMS Architecture**
`includes/database.php` = **entire backend** in one file:
```php
function db(): PDO          // singleton connection
function rows(string $sql)  // fetchAll
function row(string $sql)   // fetch one
function research_themes()  // typed query helpers
function publications()
function people_groups()    // grouped by role
function facilities_by_workflow()
function gallery_groups()
```
- **Zero migrations**: Schema created in `seed_database.php` (not in repo — run once)
- **Portable**: Single `planaria_lab.sqlite` (69KB) = entire site state
- **Backups**: One-click copy to `database/backups/` via admin
- **No Composer, no vendor/, no autoload** — deploy by dragging folder

### 4. **Admin Panel: Config-Driven CRUD (`admin/includes/tables.php`)**
Define a table once, get list/create/edit/delete free:
```php
'people' => [
  'fields' => [
    'name' => ['type' => 'text', 'required' => true],
    'role' => ['type' => 'select', 'options' => ['PI','PhD','Postdoc','Alumni']],
    'image' => ['type' => 'image', 'upload_dir' => 'people'],
    'is_current' => ['type' => 'checkbox'],
  ],
  'list_columns' => ['name', 'role', 'is_current'],
  'group_by' => 'category',  // PI / Current / Alumni
  'sort_field' => 'sort_order',
]
```
- **Image uploads**: MIME validation, unique naming (`timestamp_random.ext`), organized by table
- **Contact submissions**: Read-only list + "Reply" button → opens `mailto:` with pre-filled subject
- **Dashboard**: Live counts per section (`SELECT COUNT(*) FROM publications` etc.)

### 5. **Publication Schema Matches Real Academic Needs**
```sql
CREATE TABLE publications (
  year INT,                    -- for year-grouped display
  authors TEXT,                -- "Pramanik S.*, Bala A., Pradhan A."
  title TEXT,
  journal_info TEXT,           -- "Frontiers in Cell Dev Bio, 2024, 12:1343800"
  doi_url TEXT,
  impact_factor DECIMAL(3,1),  -- 4.2, 5.6, 8.5
  type ENUM('journal','conference','book'),
  journal_cover TEXT,          -- cover image path
  featured BOOLEAN,            -- highlight on homepage
  display_order INT
);
```
- **7 real papers (2017–2024)** seeded: planarian scRNA-seq, zebrafish Rett syndrome, PFAS-TTR binding, IL enzyme engineering, neurotrophin review
- **Display logic**: Group by year DESC, within year: featured first, then `display_order`

### 6. **Security Basics Actually Implemented**
- PDO prepared statements everywhere (no string concat)
- `e()` = `htmlspecialchars($v, ENT_QUOTES, 'UTF-8')` on all output
- CSRF tokens on all admin forms (`$_SESSION['csrf_token']`)
- Session config: `httponly`, `samesite=Strict`, 1-hour timeout, activity renewal
- `.htaccess` blocks: `includes/`, `database/`, `storage/`, `admin/includes/`
- Security headers: CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy

---

## Technical Specifications

| Layer | Detail |
|-------|--------|
| **PHP** | 8.0+ (strict types, arrow functions, nullsafe operator) |
| **Database** | SQLite 3 (`planaria_lab.sqlite`), 13 tables, indexes on sort/display fields |
| **Frontend CSS** | `site.css` (37KB), `admin.css` (8KB) — custom properties, Grid/Flex, `clamp()`, `@media (prefers-reduced-motion)` |
| **Frontend JS** | `site.js` (4KB) — mobile nav, regeneration switcher, stat counters, smooth scroll |
| **Admin JS** | Inline in `tables.php`/`edit.php` — image preview, confirm delete |
| **Images** | `assets/images/` (hero, logos, journal covers, regeneration stages) + `assets/uploads/` (user content) |
| **Email** | PHP `mail()` for contact form → admin notification (SMTP recommended for prod) |
| **Deployment** | Apache + `.htaccess` (rewrites, security headers, HTTPS redirect) |

---

## Improvements Needed

### Critical (Before Hardening for Multi-Admin Use)
| Issue | Fix |
|-------|-----|
| **Password hashing** | `admin_users.password_hash` uses custom hash — **replace with `password_hash()` / `password_verify()`** (bcrypt/argon2id) |
| **Email delivery** | `mail()` fails on many hosts — integrate **PHPMailer + SMTP** (IISER relay or SendGrid) |
| **Image processing** | No resize/compression — add **GD resize on upload** (max 1920px, 85% quality, WebP output) |
| **Admin pagination** | `records.php` loads all rows — add `LIMIT 25 OFFSET $page` + pagination links |

### High Priority (Polish & Reliability)
| Area | Recommendation |
|------|----------------|
| **Search** | Add SQLite FTS5 virtual table for `publications` (title, authors, journal) + `people` (name, topic) |
| **Publication import** | ORCID/Google Scholar/DBLP API button in admin → auto-create draft entries |
| **Content versioning** | `content_revisions` table (table_name, record_id, old_json, new_json, user_id, timestamp) |
| **Analytics** | Plausible/Matomo snippet in `layout.php` footer (opt-out friendly) |
| **Sitemap/robots** | Auto-generate `sitemap.xml` from `publications`, `research_themes`, `people`, `news` |
| **Backup rotation** | Cron: daily → keep 30, weekly → keep 12, monthly → keep 24; offsite sync (rclone to Drive/S3) |
| **Accessibility audit** | Heading order, contrast ratios (gold on navy = 4.1:1 — needs 4.5:1), focus outlines, alt text enforcement |

### Medium Priority (Developer Experience)
| Item | Why |
|------|-----|
| **Composer + PSR-4** | Autoload `includes/` classes, add PHPStan level 5, Psalm, PHP CS Fixer |
| **Testing** | Pest PHP for unit tests (query helpers, slug generation, auth) |
| **CI/CD** | GitHub Actions: lint → test → build assets → deploy to staging (Render/Railway) |
| **Env config** | `.env` for `APP_URL`, `DB_PATH`, `MAIL_*`, `ADMIN_EMAIL` — not hardcoded in `config.php` |
| **Headless API** | Slim/Laravel API resources → Astro/Next.js frontend (decouple CMS from presentation) |

---

## Verdict

**Best-in-class for "solo dev builds lab site in a weekend" category.**

The **editorial design** (regeneration timeline, stat counters, journal-cover publications) makes the science legible and beautiful. The **SQLite + config-driven admin** means zero maintenance burden — no Composer updates, no migration failures, no vendor lock-in. The **schema anticipates academic workflows** (workflow-based facilities, publication types, PI career timeline).

**Trade-off accepted:** No user roles, no revision history, no full-text search, no API. For a 5-person lab with one content editor (the PI or a PhD student), this is the right scope. The code is readable enough that the next dev *can* add those features without rewrite.

**If rebuilding for scale:** Laravel + Filament (admin) → API → Astro (frontend). But for *this* lab, *this* year? **Ship it.** The regeneration timeline alone communicates the lab's identity better than 90% of academic sites.

---

## Quick Start for Next Developer

```bash
# 1. Clone
git clone https://github.com/dhzso/web-builder
cd SP website

# 2. Permissions
chmod 755 storage/sessions database/backups assets/uploads/*/

# 3. Seed DB (run once)
php scripts/seed_database.php
# Creates admin: admin / Admin@12345  ← CHANGE IMMEDIATELY

# 4. Apache vhost → public folder = SP website/
# Enable mod_rewrite, SSL, PHP 8.2+

# 5. Admin: /admin/login.php
# 6. Edit site settings in includes/config.php (SITE_NAME, BASE_PATH, ADMIN_EMAIL)
```