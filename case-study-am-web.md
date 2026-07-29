# Case Study: HPI Lab Website — Host Pathogen Interaction Group, IISER Kolkata

**Project Type:** Academic Laboratory Website with Custom CMS  
**Client:** Dr. Amitabha Mukherjee, HPI Lab, IISER Kolkata  
**Role:** Full-stack Developer (Deepanshu Khairwal / The Setting Sun Web Solutions)  
**Timeline:** v4 delivered ~July 2026  
**Tech Stack:** PHP 8.2, SQLite 3, Vanilla JS/CSS, Apache  
**Architecture:** Custom MVC-style with generic admin CRUD engine  

---

## What Works Well

### 1. **Purpose-Built for Academic Labs**
- **13-table schema** covers every lab website need: people (PI/PhD/alumni), publications (by year/type), research themes, facilities (by workflow), gallery, news, hero slider, open positions, contact submissions
- **Generic admin CRUD** driven by `tables.php` config — add new content types without writing new PHP
- **Workflow-aware**: Facilities grouped by "Cell Culture", "Imaging", "Computational"; People by role; Publications by type (journal/conf/book)

### 2. **Security-First Implementation**
- PDO prepared statements everywhere — zero SQL injection surface
- CSRF tokens on all admin forms (session-scoped, validated on POST)
- Session hardening: `httponly`, `samesite=Strict`, 1-hour timeout, activity tracking
- `.htaccess` blocks: directory browsing, sensitive folders (`includes/`, `database/`, `storage/`, `admin/includes/`), security headers (CSP, X-Frame-Options, HSTS-ready)
- File upload validation: MIME + extension whitelist, 10MB limit, timestamped filenames, organized by category

### 3. **Editorial Design Quality**
- Clean academic aesthetic: navy/gold/blue palette, Montserrat/Open Sans via system fonts
- Hero slider with primary-slide lock (fixed "IISER Kolkata" eyebrow + CTA)
- Announcement marquee (CSS animation) — non-JS, accessible
- Animated stat counters (IntersectionObserver) — publications, themes, team, years
- Partner logo scroller (CSS animation) — DST, ANRF, IISER Kolkata

### 4. **Real Content, Practical Admin UX Thoughtfulness**
- Dashboard shows live counts per section — instant content audit
- Contact submissions: read-only list + "Mail to" button (opens `mailto:` with pre-filled reply)
- One-click SQLite backup to timestamped file in `database/backups/`
- Inline image preview + "keep existing" checkbox on edit forms

### 5. Deployment Ready**
- Single `seed_database.php` creates schema + seeds 15 publications, 4 research themes, PI + team, facilities, news
- Default admin: `admin` / `Admin@12345` (forces password change on first login)
- Runs on PHP 8.2 + `pdo_sqlite` + `gd` — minimal requirements, works on shared hosting

---

## Technical Specifications

| Layer | Detail |
|-------|--------|
| **Backend** | PHP 8.0+ (strict types), singleton PDO, helper functions (`lab_db()`, `lab_all()`, `lab_one()`, `url_for()`, `asset_url()`, `e()`) |
| **Database** | SQLite 3 (`database/lab.sqlite` ~128KB), 13 tables with indexes on `sort_order`, `is_active`, `year`, `category` |
| **Frontend** | `site.css` (31KB), `site.js` (mobile nav, Fancybox lightbox, stat counters, announcement rotator, smooth scroll) |
| **Admin CSS/JS** | `admin.css` (table/forms/layout), `tables.php` config drives all forms |
| **Image Handling** | GD library, uploads to `assets/uploads/{hero,research,people,gallery,facilities,general}/` |
| **Email** | PHP `mail()` for contact notifications (SMTP config recommended for prod) |
| **URLs** | Clean via `.htaccess` rewrite (optional) or direct `.php` |
| **Session Storage** | File-based in `storage/sessions/` (auto-cleanup via GC) |

---

## Improvements Needed

### Critical (Do Before Production)
| Issue | Fix |
|-------|-----|
| **Password hashing** | Currently uses simple hash — **must upgrade to `password_hash()` / `password_verify()`** (bcrypt/argon2) |
| **Email reliability** | `mail()` often blocked/spam — integrate PHPMailer + SMTP (SendGrid, Mailgun, or IISER mail relay) |
| **HTTPS enforcement** | `.htaccess` redirect commented — uncomment + HSTS header once SSL cert installed |
| **Admin password policy** | Add min-length, complexity, rotation reminder |

### High Priority
| Area | Recommendation |
|------|----------------|
| **Pagination** | Admin lists load all rows — add `LIMIT/OFFSET` + pagination for publications/people (will hit 100+ rows) |
| **Image optimization** | No auto-resize/compression — add GD resize on upload (max 1920px, 85% quality), generate WebP |
| **Search** | No full-text search — add SQLite FTS5 virtual table for publications/people |
| **CSRF on AJAX** | Future AJAX endpoints need token header validation |
| **Content versioning** | No revision history — add `content_revisions` table for rollback |

### Medium Priority
- **Multi-user admin**: Roles (editor vs admin), per-section permissions
- **Publication import**: ORCID/Google Scholar API sync button
- **Analytics**: Plausible/Matomo snippet in footer (privacy-friendly)
- **Sitemap.xml + robots.txt**: Auto-generate from DB
- **Backup rotation**: Cron job + retention (keep last 30 daily, 12 monthly)
- **Accessibility audit**: Heading order, color contrast (gold on navy), focus outlines, alt text enforcement

### Architectural (If Rebuilding)
- **Framework**: Move to Laravel/Slim + Blade/Twig — routing, DI, validation, migrations, testing
- **Database**: PostgreSQL for production (concurrent writes, better JSON, full-text)
- **Frontend**: Alpine.js + Tailwind (replace jQuery/Bootstrap/Fancybox) — ~90% JS reduction
- **Deployment**: GitHub Actions → VPS/Render/Railway with SQLite → Postgres migration
- **Headless option**: Decouple admin (CMS) from frontend (Astro/Next.js) via API — enables static generation + ISR

---

## Verdict

**Exceptionally complete for a solo-dev academic CMS.** The schema anticipates real lab needs (workflow-based facilities, publication types, PI profile depth), the admin engine is genuinely generic (new tables = config only), and security basics are *actually implemented* — not just documented. The design is professional without being generic.

**Main gaps are operational, not structural:** password hashing, email delivery, image optimization, pagination. Fix those 4 and it's production-hardened. For a v5 rewrite, Laravel + Filament (admin) + Livewire (frontend) would give the same flexibility with 10x less boilerplate and built-in auth/permissions/migrations/testing — but v4 is already 90% there on raw PHP.

**Best feature:** The `tables.php` config-driven admin. Adding "Teaching" or "Grants" takes 5 minutes — define fields, set permissions, done. That's the kind of flexibility academic clients actually need.