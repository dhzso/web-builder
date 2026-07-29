# HPI Lab Website — Host Pathogen Interaction Group, IISER Kolkata

**Project Type:** Academic Laboratory Website with CMS  
**Client:** Host Pathogen Interaction (HPI) Lab, IISER Kolkata  
**Tech Stack:** PHP 8.0+, SQLite 3, HTML5, CSS3, Vanilla JavaScript, Apache  
**Architecture:** Custom MVC-style with admin panel, dynamic content management  
**Status:** Production-ready (v4 — "redesigned_lab_website_4")  
**Developer:** Deepanshu Khairwal (The Setting Sun Web Solutions)  
**Last Updated:** July 2026  

---

## Overview

A full-featured academic laboratory website for the Host Pathogen Interaction Group at IISER Kolkata, led by Dr. Amitabha Mukherjee (referred to as "AM" in folder naming). The site includes a secure admin panel for content management, dynamic page rendering from SQLite database, image upload handling, contact form with email notifications, and a professional academic design.

**Repository Folder:** `AM website/redesigned_lab_website_4/` (v4 — latest)  
**Legacy Version:** `AM website/hpi_lab_php_5.4.16/` (PHP 5.4 compatible legacy)

---

## Architecture & Structure

### Directory Layout
```
AM website/redesigned_lab_website_4/
├── index.php                    # Homepage — hero slider, stats, features, announcements
├── people.php                   # Team members grouped by role (PI, PhD, Former)
├── research.php                 # Research themes + featured research images
├── publications.php             # Publications by year & type (journal/conf/book)
├── facilities.php               # Lab facilities grouped by workflow
├── gallery.php                  # Photo gallery by category
├── news.php                     # Announcements with category filter
├── contact.php                  # Contact form + info + Google Maps
├── profile.php                  # PI detailed profile page
├── lab-feature.php              # Individual lab feature detail
├── faculty-profile.php          # Faculty profile detail
├── features.php                 # All lab features listing
├── .htaccess                    # Security headers, rewrite rules, HTTPS redirect
├── site_manual.md               # Comprehensive admin/user manual
├── admin/
│   ├── index.php               # Dashboard with content counts
│   ├── login.php               # Secure login with CSRF
│   ├── logout.php              # Session destruction
│   ├── password.php            # Password change
│   ├── records.php             # Generic CRUD list view
│   ├── edit.php                # Generic create/edit form
│   ├── delete.php              # Delete confirmation
│   ├── backup.php              # One-click database backup
│   └── includes/
│       ├── bootstrap.php       # Auth, CSRF, session config
│       ├── layout.php          # Admin header/footer
│       └── tables.php          # Table configuration (fields, types, permissions)
├── assets/
│   ├── css/
│   │   ├── site.css            # Main stylesheet (31KB)
│   │   └── admin.css           # Admin panel styles
│   ├── js/
│   │   └── site.js             # Mobile nav, lightbox, smooth scroll
│   ├── images/
│   │   ├── logo_hpi.png        # HPI Lab logo
│   │   ├── process_cycle.png   # Research process illustration
│   │   ├── process_cycle_2.png # Alt process illustration
│   │   ├── get-in-touch.jpg    # Contact section hero
│   │   └── Iiser-Kolkata-Logo-Vector.png
│   ├── uploads/                # User-uploaded content (organized by type)
│   │   ├── hero/               # Hero slider images
│   │   ├── research/           # Research theme images
│   │   ├── people/             # Team member photos
│   │   ├── gallery/            # Gallery images
│   │   ├── facilities/         # Facility photos
│   │   └── general/            # Publication covers, misc
│   └── vendor/
│       └── fancybox/           # jQuery lightbox library
├── includes/
│   ├── config.php              # Constants, helpers (url_for, asset_url, e())
│   ├── database.php            # PDO connection, lab_db(), lab_all(), lab_one()
│   ├── layout.php              # render_header(), render_footer(), nav data
│   └── data.php                # Query functions for each content type
├── scripts/
│   └── seed_database.php       # Database initialization + sample data
├── database/
│   ├── lab.sqlite              # SQLite database (128KB)
│   └── backups/                # Timestamped backups
└── storage/
    └── sessions/               # PHP session files
```

---

## Database Schema (13 Tables)

| Table | Purpose | Key Fields |
|-------|---------|------------|
| `announcements` | News, publications, updates | title, body, category, event_date, is_active, show_on_home |
| `hero_slides` | Homepage slider | title, subtitle, image, link_label, link_url, is_primary, sort_order |
| `lab_features` | Highlighted research capabilities | title, summary, body, image, link_url, is_active, sort_order |
| `people` | Lab members | name, role, group_name, topic, publications, image, email, is_current |
| `pi_profile` | PI detailed profile | name, designation, lab, bio, education, research_interests, awards |
| `research_themes` | Major research areas | title, summary, body, image, doi_link, sort_order |
| `research_features` | Specific research projects | title, caption, image, doi_link, sort_order |
| `publications` | Academic publications | year, authors, title, journal_info, doi_url, type, journal_cover |
| `gallery_items` | Photo gallery | title, caption, group_name, image, is_active, sort_order |
| `facility_items` | Lab equipment/facilities | title, caption, image, sort_order |
| `open_positions` | Job openings | title, body, link_url, is_active, sort_order |
| `contact_submissions` | Contact form entries | name, email, subject, message, created_at |
| `admin_users` | Admin accounts | username, password_hash, display_name |

---

## Core PHP Functions

### Database Layer (`includes/database.php`)
```php
function lab_db(): PDO           // Singleton PDO connection
function lab_all(string $sql, array $params = []): array    // Fetch all
function lab_one(string $sql, array $params = []): ?array   // Fetch one
```

### Helpers (`includes/config.php`)
```php
function url_for(string $path = ''): string      // Site-relative URLs
function asset_url(string $path): string         // Asset URLs
function e(?string $value): string               // htmlspecialchars wrapper
function active_class(string $page, string $currentPage): string
```

### Data Access (`includes/data.php`)
```php
function lab_hero_slides(): array
function lab_home_announcements(): array
function lab_home_stats(): array
function lab_features(): array
function research_themes(): array
function latest_news(int $limit = 6): array
function open_position(): ?array
function publications(): array
function people_groups(): array      // Grouped by role
function facilities_by_workflow(): array
function gallery_groups(): array
```

---

## Frontend Pages Detail

### 1. **Homepage (`index.php`)**
- **Hero Slider**: 3-5 slides from `hero_slides` table, primary slide fixed with "IISER Kolkata" eyebrow
- **Announcement Strip**: Rotating marquee of latest announcements (max 16)
- **Research Focus Intro**: Static editorial content + process cycle image
- **Statistics Band**: 4 dynamic counters (publications, research themes, team members, years active)
- **Lab Features**: 3 featured cards linking to `lab-feature.php?id=X`

### 2. **People (`people.php`)**
- Groups: Principal Investigator → Current Members → Former Members → Alumni
- Cards: Photo, name, role, research topic, profile link
- `is_current` flag controls homepage stats count

### 3. **Research (`research.php`)**
- Research themes (title, summary, body, image, DOI link)
- Featured research images with captions and DOI links
- Sortable by `sort_order`

### 4. **Publications (`publications.php`)**
- Grouped by year (descending)
- Filtered by type: Journal Article / Conference Paper / Book Chapter
- Fields: Authors, title, journal info, DOI, impact factor, journal cover image
- Featured flag for highlighting

### 5. **Facilities (`facilities.php`)**
- Grouped by workflow category (e.g., "Cell Culture", "Imaging", "Computational")
- Grid layout with image, title, caption

### 6. **Gallery (`gallery.php`)**
- Grouped by category (`group_name`)
- Lightbox via Fancybox
- Sortable, filterable

### 7. **News (`news.php`)**
- All announcements with date, category badge
- Filter by category (News, Publication, etc.)
- `show_on_home` controls announcement strip

### 8. **Contact (`contact.php`)**
- Form: Name, Email, Subject, Message
- Server-side validation
- Saves to `contact_submissions` table
- Sends email notification via PHP `mail()`
- Contact info + Google Maps embed

### 9. **PI Profile (`profile.php`)**
- Detailed PI page: bio, education, research interests, awards, funded projects, image

### 10. **Lab Feature Detail (`lab-feature.php`)**
- Full article view for individual lab features
- Reads `id` from query string

---

## Admin Panel Features

### Authentication (`admin/includes/bootstrap.php`)
- Session-based with 1-hour timeout (`SESSION_TIMEOUT = 3600`)
- CSRF token per session (`$_SESSION['csrf_token']`)
- Secure session config: `httponly`, `samesite=Strict`, `secure` (if HTTPS)
- Password hashing: Currently simple hash — **recommend upgrade to `password_hash()`**

### Dashboard (`admin/index.php`)
- Card grid showing record counts for each content section
- Quick "Add new" links
- Backup button in header

### Generic CRUD (`admin/records.php`, `edit.php`, `delete.php`)
- Driven by `admin/includes/tables.php` configuration
- Dynamic form generation based on field definitions
- Field types: `text`, `textarea`, `checkbox`, `image`, `email`, `url`, `number`, `select`
- Image upload with validation (JPG, PNG, GIF, WebP, max 10MB)
- Unique filename generation: `{timestamp}_{random}.{ext}`

### Table Configuration (`admin/includes/tables.php`)
Each table defines:
```php
'table_name' => [
    'label' => 'Display Name',
    'fields' => [
        'field_name' => ['label' => 'Label', 'type' => 'text|textarea|image|...', 'required' => true],
    ],
    'list_columns' => ['id', 'title', 'is_active'],
    'sort_order' => 'sort_order',
    'allow_create' => true,
    'allow_edit' => true,
]
```

### Special: Contact Submissions
- Read-only view
- "Mail to" button opens `mailto:` with submitter's email
- Pre-fills subject: `Re: [original subject]`

### Backup (`admin/backup.php`)
- One-click: copies `lab.sqlite` to `database/backups/lab_YYYY-MM-DD_HH-MM-SS.sqlite`
- Automatic backup on seed script run

---

## Security Implementation

| Measure | Implementation |
|---------|----------------|
| **CSRF Protection** | Token in session, validated on all POST forms |
| **SQL Injection** | All queries use prepared statements (PDO) |
| **XSS Prevention** | `e()` wrapper (`htmlspecialchars`) on all output |
| **File Upload** | MIME validation, extension whitelist, size limit, secure naming |
| **Session Security** | HttpOnly, SameSite=Strict, 1hr timeout, activity tracking |
| **Directory Protection** | `.htaccess` blocks access to `includes/`, `database/`, `storage/`, `admin/includes/` |
| **Security Headers** | X-Content-Type-Options, X-Frame-Options, X-XSS-Protection, Referrer-Policy, CSP |
| **HTTPS Redirect** | In `.htaccess` (commented — enable for production) |
| **Password Storage** | Currently basic hash — **upgrade to bcrypt/argon2** |

---

## CSS Design System (`assets/css/site.css`)

### Color Palette
```css
--primary: #1a365d;        /* Deep navy */
--secondary: #b7832d;      /* Gold/amber */
--accent: #4299e1;         /* Light blue */
--bg: #ffffff;             /* White */
--bg-alt: #f7fafc;         /* Light gray */
--text: #2d3748;           /* Dark gray */
```

### Typography
- System font stack (Inter, Roboto, -apple-system, sans-serif)
- Responsive clamp() scaling for headings
- Editorial style with "eyebrow" kickers

### Layout Components
- **Grid**: CSS Grid + Flexbox hybrid
- **Breakpoints**: Mobile-first (480px, 768px, 1024px, 1200px)
- **Hero**: Full-viewport with overlay content
- **Cards**: Consistent `.research-card`, `.stat-card`, `.person-card`
- **Forms**: Styled inputs, validation states, file upload preview
- **Tables**: Admin list views with hover, actions column

### Key Visual Features
- Hero slider with cross-fade transitions
- Announcement marquee (CSS animation)
- Stat counters with JS animation (`data-stats-counter`)
- Partner logo scroller (CSS scroll animation)
- Lightbox via Fancybox (gallery, research images)

---

## JavaScript (`assets/js/site.js`)

### Features
- **Mobile Navigation**: Hamburger toggle, body scroll lock
- **Image Lightbox**: Fancybox initialization for `.gallery a`, `.research-feature a`
- **Smooth Scroll**: Anchor links with offset for fixed header
- **Stat Counter Animation**: IntersectionObserver triggers count-up
- **Announcement Rotator**: Auto-advance marquee items
- **Regeneration Stage Switcher** (if applicable): Tab-like stage navigation

---

## Deployment & Server Requirements

### Requirements
- **PHP**: 8.0+ (tested on 8.2.12)
- **Extensions**: `pdo_sqlite`, `gd` (image processing), `session`, `mbstring`
- **Web Server**: Apache (`.htaccess` required) or Nginx (config translation needed)
- **Write Permissions**:
  - `database/` (for `lab.sqlite`)
  - `storage/sessions/`
  - `assets/uploads/` and all subdirectories

### Apache `.htaccess` Highlights
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

# Clean URLs (optional)
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php [QSA,L]

# HTTPS redirect (uncomment for production)
# RewriteCond %{HTTPS} off
# RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

### Installation Steps
1. Extract files to web root
2. Set permissions (755 dirs, 644 files, 666 database/lab.sqlite)
3. Run `php scripts/seed_database.php` to initialize DB + admin user
4. Access `/admin/login.php` — default: `admin` / `Admin@12345`
5. **Immediately change admin password**
6. Configure HTTPS and uncomment redirect in `.htaccess`
7. Set up cron for daily backups

### Default Admin Credentials
```
Username: admin
Password: Admin@12345
```

---

## Seed Database Script (`scripts/seed_database.php`)

### Creates All Tables + Sample Data
- **Admin User**: `admin` / `Admin@12345` (hashed)
- **Hero Slides**: 3 sample slides with images
- **Research Themes**: 4 themes (Influenza, Campylobacter, Mucosal Immunology, Vector Delivery)
- **Lab Features**: 3 features (Live Vector, Bacterial Vesicles, Secretion Systems)
- **People**: PI + 2 current + 1 former member
- **Publications**: 15+ real publications from the lab (2017-2024)
- **Facilities**: 4 workflow-based facilities
- **News**: 3 sample announcements
- **Gallery**: Placeholder structure

### Publications Seeded (Sample)
| Year | Title | Journal | Type |
|------|-------|---------|------|
| 2024 | Mucosal immunity... | Frontiers in Immunology | Journal |
| 2023 | Live vector delivery... | Vaccines | Journal |
| 2022 | Campylobacter jejuni... | PLoS Pathogens | Journal |
| 2021 | Bacterial extracellular vesicles... | Nature Communications | Journal |
| 2019 | Secretion systems... | mBio | Journal |

---

## Maintenance & Operations

### Daily
- Monitor error logs (`/var/log/apache2/error.log`)
- Check contact form submissions in admin

### Weekly
- Review/approve content
- Verify backup files exist in `database/backups/`

### Monthly
- Clean old backups (keep last 30)
- Review security headers, PHP version
- Test contact form end-to-end

### Quarterly
- Update PHP and system packages
- Rotate admin password
- Test full backup restore procedure
- Review user access logs

---

## Known Issues & Technical Debt

1. **Password Hashing**: Uses simple hash — must upgrade to `password_hash()`/`password_verify()`
2. **No Pagination**: Admin lists load all records — will need pagination for scale
3. **Image Optimization**: No automatic resizing/compression on upload
4. **Email Reliability**: Uses PHP `mail()` — configure SMTP for production
5. **CSRF on AJAX**: Not implemented for future AJAX endpoints
6. **Search**: No full-text search for publications/people
7. **Multi-language**: Single language only (English)

---

## Customization Guide

### Branding
- Update `SITE_NAME`, `SITE_TAGLINE` in `includes/config.php`
- Replace logos in `assets/images/`
- Modify CSS custom properties in `assets/css/site.css`

### Content Structure
- Edit navigation in `includes/layout.php` (`$navItems` array)
- Adjust table configs in `admin/includes/tables.php`
- Add/remove content sections via `data.php` functions

### Design
- Modify CSS variables for theming
- Adjust breakpoints in `site.css`
- Customize card layouts, hero styles

---

## Credits & Attribution

- **Developer**: Deepanshu Khairwal (The Setting Sun Web Solutions)
- **Client**: Host Pathogen Interaction Lab, IISER Kolkata
- **PI**: Dr. Amitabha Mukherjee
- **Institution**: IISER Kolkata (Indian Institute of Science Education and Research)
- **Icons**: Custom SVG + system fonts
- **Libraries**: Fancybox (jQuery lightbox), PHP PDO, SQLite
- **License**: Custom — contact developer for reuse

---

*Document generated: July 2026*  
*Repository: https://github.com/dhzso/web-builder*  
*Project Folder: `AM website/redesigned_lab_website_4/`*