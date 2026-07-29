# Comprehensive Lab Website Reproduction Prompt

This prompt provides detailed instructions to reproduce a complete academic lab website.

## Overview

Create a modern, responsive academic lab website with the following characteristics:
- PHP-based backend with SQLite database
- Admin panel for content management
- Multiple content sections: Home, People, Research, Publications, Facilities, Gallery, News, Contact
- Image upload functionality
- Contact form with email notifications
- Professional academic design
- Mobile-responsive layout

## Technical Stack

- **Backend**: PHP 8.0+ with strict types
- **Database**: SQLite 3
- **Frontend**: HTML5, CSS3, vanilla JavaScript
- **Image Handling**: GD library for image processing
- **Security**: CSRF protection, secure session management, password hashing

## Database Schema

Create the following tables in SQLite:

### 1. announcements
```sql
CREATE TABLE announcements (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    body TEXT,
    category TEXT NOT NULL DEFAULT 'News',
    event_date TEXT,
    is_active INTEGER NOT NULL DEFAULT 1,
    show_on_home INTEGER NOT NULL DEFAULT 1
);
```

### 2. hero_slides
```sql
CREATE TABLE hero_slides (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT,
    subtitle TEXT,
    image TEXT NOT NULL,
    link_label TEXT,
    link_url TEXT,
    is_primary INTEGER NOT NULL DEFAULT 0,
    is_active INTEGER NOT NULL DEFAULT 1,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 3. lab_features
```sql
CREATE TABLE lab_features (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    summary TEXT NOT NULL,
    body TEXT,
    image TEXT,
    link_url TEXT,
    is_active INTEGER NOT NULL DEFAULT 1,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 4. people
```sql
CREATE TABLE people (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    role TEXT NOT NULL,
    group_name TEXT NOT NULL,
    topic TEXT,
    publications TEXT,
    current_position TEXT,
    image TEXT,
    email TEXT,
    profile_link TEXT,
    is_current INTEGER NOT NULL DEFAULT 1,
    group_order INTEGER NOT NULL DEFAULT 0,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 5. pi_profile
```sql
CREATE TABLE pi_profile (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    designation TEXT NOT NULL,
    lab TEXT NOT NULL,
    bio TEXT,
    education TEXT,
    research_interests TEXT,
    awards TEXT,
    funded_projects TEXT,
    image TEXT,
    is_active INTEGER NOT NULL DEFAULT 1,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 6. research_themes
```sql
CREATE TABLE research_themes (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    summary TEXT NOT NULL,
    body TEXT,
    image TEXT,
    doi_link TEXT,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 7. research_features
```sql
CREATE TABLE research_features (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    caption TEXT,
    image TEXT NOT NULL,
    doi_link TEXT,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 8. publications
```sql
CREATE TABLE publications (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    year INTEGER NOT NULL,
    authors TEXT NOT NULL,
    title TEXT NOT NULL,
    journal_info TEXT NOT NULL,
    doi_url TEXT,
    type TEXT NOT NULL DEFAULT 'journal',
    journal_cover TEXT
);
```

### 9. gallery_items
```sql
CREATE TABLE gallery_items (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    caption TEXT,
    group_name TEXT NOT NULL DEFAULT 'Gallery',
    image TEXT NOT NULL,
    is_active INTEGER NOT NULL DEFAULT 1,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 10. facility_items
```sql
CREATE TABLE facility_items (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    caption TEXT,
    image TEXT NOT NULL,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 11. open_positions
```sql
CREATE TABLE open_positions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    title TEXT NOT NULL,
    body TEXT NOT NULL,
    link_url TEXT,
    is_active INTEGER NOT NULL DEFAULT 1,
    sort_order INTEGER NOT NULL DEFAULT 0
);
```

### 12. contact_submissions
```sql
CREATE TABLE contact_submissions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    email TEXT NOT NULL,
    subject TEXT NOT NULL,
    message TEXT NOT NULL,
    created_at TEXT NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

### 13. admin_users
```sql
CREATE TABLE admin_users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL UNIQUE,
    password_hash TEXT NOT NULL,
    display_name TEXT NOT NULL,
    created_at TEXT NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## Directory Structure

```
/
├── admin/
│   ├── includes/
│   │   ├── bootstrap.php
│   │   ├── layout.php
│   │   └── tables.php
│   ├── index.php
│   ├── login.php
│   ├── logout.php
│   ├── password.php
│   ├── records.php
│   ├── edit.php
│   ├── delete.php
│   └── backup.php
├── assets/
│   ├── css/
│   │   ├── site.css
│   │   └── admin.css
│   ├── js/
│   │   └── site.js
│   ├── images/
│   └── uploads/
│       ├── hero/
│       ├── research/
│       ├── people/
│       ├── gallery/
│       └── facilities/
├── includes/
│   ├── config.php
│   ├── database.php
│   ├── layout.php
│   └── data.php
├── scripts/
│   └── seed_database.php
├── database/
│   └── lab.sqlite
├── storage/
│   └── sessions/
├── index.php
├── people.php
├── research.php
├── publications.php
├── facilities.php
├── gallery.php
├── news.php
├── contact.php
├── profile.php
├── lab-feature.php
├── faculty-profile.php
└── .htaccess
```

## Core PHP Functions

### Database Functions (includes/database.php)

```php
function lab_db(): PDO
{
    static $pdo = null;
    if ($pdo instanceof PDO) {
        return $pdo;
    }
    $dbFile = SITE_ROOT . '/database/lab.sqlite';
    $pdo = new PDO('sqlite:' . $dbFile);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $pdo->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
    return $pdo;
}

function lab_all(string $sql, array $params = []): array
{
    $statement = lab_db()->prepare($sql);
    $statement->execute($params);
    return $statement->fetchAll();
}

function lab_one(string $sql, array $params = []): ?array
{
    $statement = lab_db()->prepare($sql);
    $statement->execute($params);
    $row = $statement->fetch();
    return $row === false ? null : $row;
}
```

### Helper Functions (includes/config.php)

```php
function url_for(string $path = ''): string
{
    $path = ltrim($path, '/');
    return BASE_PATH . ($path === '' ? '/' : '/' . $path);
}

function asset_url(string $path): string
{
    return url_for('assets/' . ltrim($path, '/'));
}

function e(?string $value): string
{
    return htmlspecialchars((string) $value, ENT_QUOTES, 'UTF-8');
}

function active_class(string $page, string $currentPage): string
{
    return $page === $currentPage ? ' is-active' : '';
}
```

## Admin Panel Implementation

### Authentication System

Implement secure admin authentication with:
- Session-based login with 1-hour timeout
- CSRF token protection for all forms
- Password hashing using password_hash()
- Secure session configuration (httponly, samesite)

### Admin Table Configuration

Create a configuration system in `admin/includes/tables.php` that defines:
- Table names and labels
- Field definitions with types (text, textarea, checkbox, image, email, url, number, select)
- List columns for table views
- Sort orders
- Permissions (allow_create, allow_edit)

### Image Upload System

Implement secure image upload functionality:
- Validate file types (JPG, PNG, GIF, WebP)
- Generate unique filenames with timestamps
- Organize uploads by category (hero, research, people, gallery, facilities)
- Set proper file permissions (0644)
- Handle existing image retention

## Frontend Pages

### 1. Home Page (index.php)
- Hero slider with 3-5 slides
- Statistics section (publications, research themes, current members)
- Featured lab features (3 items)
- Recent announcements
- Responsive navigation

### 2. People Page (people.php)
- Group people by category (PI, PhD Scholars, Former Members)
- Display photos, names, roles, research topics
- Link to individual profiles
- Distinguish current vs former members

### 3. Research Page (research.php)
- Research themes with summaries
- Featured research images with captions
- DOI links to publications
- Sort by research priority

### 4. Publications Page (publications.php)
- Group publications by year
- Display authors, title, journal info
- DOI links
- Filter by type (journal, conference, book)
- Journal cover images

### 5. Facilities Page (facilities.php)
- Grid layout of facility images
- Captions for each facility
- Sort by priority

### 6. Gallery Page (gallery.php)
- Group gallery items by category
- Image grid with captions
- Lightbox/modal for image viewing
- Filter by group

### 7. News Page (news.php)
- All announcements with dates
- Filter by category (News, Publication)
- Event date display
- Active/inactive status

### 8. Contact Page (contact.php)
- Contact form with name, email, subject, message
- Form validation
- Save to database
- Email notification to admin
- Contact information display
- Google Maps integration

### 9. PI Profile Page (profile.php)
- Detailed PI information
- Bio, education, research interests
- Awards and funded projects
- Photo

## CSS Design System

### Color Scheme
- Primary: Deep blue/navy (#1a365d)
- Secondary: Gold/amber (#b7832d)
- Accent: Light blue (#4299e1)
- Background: White (#ffffff) and light gray (#f7fafc)
- Text: Dark gray (#2d3748)

### Typography
- Headings: Modern sans-serif (Inter, Roboto, or system fonts)
- Body: Readable sans-serif
- Sizes: Responsive scaling

### Layout Components
- Responsive grid system
- Flexbox for navigation and cards
- Mobile-first approach
- Tablet and desktop breakpoints

### Key CSS Features
- Smooth transitions and hover effects
- Card-based content layout
- Hero section with overlay
- Footer with multiple columns
- Admin panel with table views
- Form styling with validation states

## JavaScript Functionality

### Site JavaScript (assets/js/site.js)
- Mobile navigation toggle
- Image modal/lightbox for gallery
- Smooth scrolling
- Form validation enhancement
- Dynamic content loading (if needed)

## Security Features

### CSRF Protection
- Generate unique tokens per session
- Validate tokens on form submissions
- Include tokens in all admin forms

### Session Security
- Secure session configuration
- Session timeout (1 hour)
- Session storage in dedicated directory
- HttpOnly and SameSite cookies

### Input Validation
- Server-side validation for all inputs
- Output escaping with htmlspecialchars()
- SQL injection prevention with prepared statements
- File upload validation

### Password Security
- Use password_hash() with default algorithm
- Never store plain text passwords
- Implement secure password change functionality

## Contact Form Implementation

### Form Submission
```php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $name = $_POST['name'] ?? '';
    $email = $_POST['email'] ?? '';
    $subject = $_POST['subject'] ?? '';
    $message = $_POST['message'] ?? '';
    
    // Validate inputs
    if ($name && $email && $subject && $message) {
        // Save to database
        $stmt = $pdo->prepare('INSERT INTO contact_submissions (name, email, subject, message) VALUES (:name, :email, :subject, :message)');
        $stmt->execute(['name' => $name, 'email' => $email, 'subject' => $subject, 'message' => $message]);
        
        // Send email notification
        $to = 'admin@example.com';
        $emailSubject = "Contact Form Submission: $subject";
        $emailBody = "Name: $name\nEmail: $email\nSubject: $subject\n\nMessage:\n$message";
        $headers = "From: $email\r\nReply-To: $email\r\n";
        mail($to, $emailSubject, $emailBody, $headers);
    }
}
```

## Admin Panel Features

### Dashboard
- Overview of all content sections
- Record counts for each table
- Quick links to manage content
- Admin notes and documentation

### Records Management
- List view with pagination
- Sort by relevant columns
- Search functionality
- Bulk actions (delete)

### Edit/Create Forms
- Dynamic form generation based on table configuration
- Image upload with preview
- Validation feedback
- Save and cancel actions

### Contact Submissions
- Read-only view of form submissions
- "Mail to" button instead of save button
- Opens email client with submitter's email
- Pre-fills subject line with "Re: [original subject]"

## Database Seeding

Create a seed script that:
- Creates all tables
- Inserts sample data for testing
- Creates default admin user (username: admin, password: Admin@12345)
- Populates with realistic lab data
- Includes sample images (placeholder paths)

## Deployment Considerations

### Server Requirements
- PHP 8.0 or higher
- SQLite 3 support
- GD library for image processing
- Write permissions for:
  - database/ directory
  - storage/sessions/ directory
  - assets/uploads/ subdirectories

### .htaccess Configuration
```apache
Options -Indexes
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php [QSA,L]
</IfModule>
```

### Backup Strategy
- Regular database backups
- Backup uploaded images
- Version control for code
- Document configuration changes

## Customization Guidelines

### Branding
- Update SITE_NAME and SITE_TAGLINE in config.php
- Replace logos in assets/images/
- Customize color scheme in CSS
- Update contact information in data.php

### Content Structure
- Modify navigation items in data.php
- Adjust table configurations in admin/includes/tables.php
- Customize field labels and types
- Add or remove content sections

### Design Modifications
- Modify CSS variables for theming
- Adjust layout breakpoints
- Customize card designs
- Update typography settings

## Testing Checklist

- [ ] All database tables created correctly
- [ ] Admin login/logout works
- [ ] CRUD operations for all content types
- [ ] Image upload functionality
- [ ] Contact form submission and email
- [ ] Responsive design on mobile/tablet/desktop
- [ ] Navigation works across all pages
- [ ] CSRF protection active
- [ ] Session timeout works
- [ ] File permissions correct
- [ ] Database backup/restore works

## Maintenance Tasks

- Regular database backups
- Monitor storage usage for uploads
- Update PHP dependencies
- Review security logs
- Test contact form functionality
- Update content regularly
- Monitor for broken links

## Performance Optimization

- Implement database indexing
- Optimize image sizes
- Enable browser caching
- Minify CSS and JavaScript
- Use lazy loading for images
- Implement pagination for large datasets

This comprehensive prompt provides all necessary information to reproduce a fully functional academic lab website with admin panel, content management, and modern design features.
