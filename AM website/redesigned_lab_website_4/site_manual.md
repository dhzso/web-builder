# HPI Lab Website - Site Manual

## Overview

This is a complete laboratory website with a dynamic content management system (CMS) built with PHP and SQLite. The site includes public-facing pages for lab information, research, publications, and contact, along with a secure admin panel for content management.

## Table of Contents

1. [System Requirements](#system-requirements)
2. [Installation](#installation)
3. [Admin Panel Access](#admin-panel-access)
4. [Content Management](#content-management)
5. [Security Features](#security-features)
6. [Database Management](#database-management)
7. [Backup and Recovery](#backup-and-recovery)
8. [Troubleshooting](#troubleshooting)
9. [Contact and Support](#contact-and-support)

## System Requirements

- **PHP Version**: 8.0 or higher (currently running PHP 8.2.12)
- **PHP Extensions**: `pdo_sqlite`, `gd` (for image processing)
- **Web Server**: Apache (recommended) or Nginx
- **Database**: SQLite (included)
- **Disk Space**: Minimum 500MB (more for image uploads)

## Installation

### Local Development

1. Extract the website files to your web server directory
2. Ensure PHP is installed with required extensions
3. Set appropriate file permissions:
   ```bash
   chmod 755 -R /path/to/website
   chmod 644 database/lab.sqlite
   chmod 755 storage/
   ```
4. Access the site via your web browser

### Production Deployment

1. Upload all files to your web server
2. Configure HTTPS (SSL certificate)
3. Update `.htaccess` to uncomment HTTPS redirect
4. Change default admin password immediately
5. Set up regular database backups
6. Configure SMTP for email notifications (optional)

## Admin Panel Access

### Login Credentials

**Default Login:**
```
Username: admin
Password: Admin@12345
```

**Important:** Change this password immediately after first login!

### Access URL

```
https://yourdomain.com/admin/login.php
```

### Admin Panel Features

The admin panel allows you to manage:

- **Announcements**: News, publications, and updates
- **Home Slider**: Hero images and text
- **Lab Features**: Highlighted research areas
- **People**: Lab members and their profiles
- **Research Themes**: Main research focus areas
- **Featured Research**: Specific research projects
- **Publications**: Academic publications list
- **Gallery**: Lab photos and activities
- **Facilities**: Equipment and lab facilities
- **Open Positions**: Job opportunities
- **Contact Submissions**: View form submissions

### Navigation

- **Dashboard**: Overview of content sections with counts
- **Sections**: Click on any section to manage its content
- **Add/Edit/Delete**: Standard CRUD operations for all content
- **Backup**: One-click database backup button in header

## Content Management

### Adding New Content

1. Navigate to the desired section (e.g., Announcements)
2. Click "Add new" button
3. Fill in required fields (marked with asterisk)
4. Upload images if required
5. Click "Save"

### Editing Content

1. Navigate to the desired section
2. Click "Edit" or "View" button for the item
3. Make your changes
4. Click "Save"

### Deleting Content

1. Navigate to the desired section
2. Click "Delete" button for the item
3. Confirm deletion

### Image Upload Guidelines

- **Supported Formats**: JPG, PNG, GIF, WebP
- **Maximum Size**: 10MB per image
- **Naming**: Files are automatically renamed with timestamps
- **Storage**: Images stored in `assets/uploads/` subdirectories
- **Paths**: Use relative paths like `uploads/research/image.jpg`

### Content Sections Detail

#### Announcements
- **Title**: Announcement headline
- **Body**: Full announcement text
- **Category**: News, Publication, or custom
- **Event Date**: Date for sorting (latest appears first)
- **Show on Home**: Display in homepage announcement bar
- **Active**: Enable/disable visibility
- **Note**: Announcements are automatically sorted by event date (latest first)

#### Hero Slides
- **Title**: Slide headline
- **Subtitle**: Supporting text
- **Image**: Background image (required)
- **Link Label**: Button text
- **Link URL**: Destination URL
- **Primary Slide**: Fixed homepage text overlay
- **Active**: Enable/disable

#### Lab Features
- **Title**: Feature name
- **Summary**: Brief description
- **Body**: Full article content (clickable cards link to detail page)
- **Image**: Feature image
- **Link URL**: External link (optional)
- **Active**: Enable/disable
- **Sort Order**: Display order

#### People
- **Name**: Full name
- **Role**: Position/title
- **Group**: Team category
- **Research Topic**: Current research focus
- **Publications**: Key publications
- **Current Position**: Current job title
- **Image**: Profile photo
- **Email**: Contact email
- **Profile Link**: External profile URL
- **Current Member**: Check if currently active (counts in homepage stats)
- **Group Order**: Section display order
- **Sort Order**: Individual display order

#### Research Themes
- **Title**: Research theme name
- **Summary**: Brief description
- **Body**: Detailed content
- **Image**: Theme image
- **DOI Link**: Digital Object Identifier (optional)
- **Sort Order**: Display order

#### Publications
- **Year**: Publication year (required)
- **Authors**: Author names (required)
- **Title**: Publication title (required)
- **Journal / Publication Info**: Journal name, volume, pages, impact factor (required)
- **DOI / Link**: Digital Object Identifier or external link (optional)
- **Type**: Publication type - Journal Article, Conference Paper, or Book Chapter (required)
- **Note**: Publications are automatically sorted by year (latest first) and displayed in separate sections by type

#### Gallery
- **Title**: Image caption
- **Group**: Category/album
- **Image**: Photo file
- **Active**: Enable/disable
- **Sort Order**: Display order

#### Facilities
- **Title**: Facility name
- **Caption**: Description
- **Image**: Facility photo
- **Sort Order**: Display order

#### Open Positions
- **Title**: Position name
- **Body**: Position description
- **Link URL**: Application link
- **Active**: Enable/disable
- **Sort Order**: Display order

## Security Features

### Implemented Security Measures

1. **CSRF Protection**: All forms use secure tokens to prevent cross-site request forgery
2. **Session Security**:
   - HTTP-only cookies
   - Strict SameSite policy
   - 1-hour session timeout
   - Activity tracking
3. **SQL Injection Protection**: All database queries use prepared statements
4. **XSS Protection**: Output is properly escaped
5. **File Upload Security**:
   - Image type validation
   - File size limits
   - Secure file naming
   - Permission restrictions
6. **Directory Protection**: Sensitive directories protected via .htaccess
7. **Security Headers**: HTTP security headers configured
8. **Password Storage**: Passwords hashed (currently using simple hash - upgrade to bcrypt recommended)

### Security Best Practices

1. **Change Default Password**: Immediately change admin password
2. **Use HTTPS**: Enable SSL/TLS for production
3. **Regular Backups**: Schedule daily database backups
4. **Update PHP**: Keep PHP version updated
5. **Monitor Logs**: Check error logs regularly
6. **Limit Access**: Restrict admin panel to trusted IPs if possible
7. **Strong Passwords**: Use complex passwords with special characters
8. **Session Management**: Log out after use, don't share credentials

### Security Configuration

The `.htaccess` file includes:
- Security headers (XSS protection, frame options, etc.)
- Directory browsing disabled
- Sensitive file protection
- HTTPS redirect (commented out - enable for production)

## Database Management

### Database Structure

The site uses SQLite database (`database/lab.sqlite`) with the following tables:

- `announcements` - News and updates
- `hero_slides` - Homepage slider images
- `lab_features` - Featured lab capabilities
- `people` - Lab members
- `research_themes` - Research focus areas
- `research_features` - Specific research projects
- `publications` - Academic publications
- `gallery_items` - Lab photos
- `facility_items` - Lab equipment
- `open_positions` - Job openings
- `admin_users` - Admin accounts
- `contact_submissions` - Contact form submissions

### Manual Database Access

Use DB Browser for SQLite or similar tool to open:
```
database/lab.sqlite
```

### Rebuilding Database

To reset the database to initial seeded data:
```bash
php scripts/seed_database.php
```

**Warning:** This will overwrite all content including admin password changes!

### Database Schema

The database schema is defined in `scripts/seed_database.php`. To modify the schema:
1. Update the CREATE TABLE statements
2. Update the seed data arrays
3. Run the seed script

## Backup and Recovery

### Automatic Backup

The admin panel includes a one-click backup button:
1. Log in to admin panel
2. Click "Backup" in the header
3. Backup is saved to `database/backups/` with timestamp

### Manual Backup

```bash
# Copy database file
cp database/lab.sqlite database/lab.sqlite.backup

# Or create timestamped backup
cp database/lab.sqlite database/backups/lab_$(date +%Y%m%d_%H%M%S).sqlite
```

### Backup Schedule

For production, set up automated daily backups using cron:
```bash
# Add to crontab
0 2 * * * cp /path/to/database/lab.sqlite /path/to/backups/lab_$(date +\%Y\%m\%d).sqlite
```

### Recovery

To restore from backup:
```bash
cp database/backups/lab_YYYYMMDD_HHMMSS.sqlite database/lab.sqlite
```

## Troubleshooting

### Common Issues

**Issue: Cannot login to admin panel**
- Solution: Clear browser cookies, check username/password, verify database exists

**Issue: Images not displaying**
- Solution: Check file permissions, verify upload directory exists, check image paths in database

**Issue: Database errors**
- Solution: Verify SQLite extension is enabled, check file permissions on database file

**Issue: Email not sending**
- Solution: Configure SMTP settings, check PHP mail configuration, verify email address

**Issue: Session timeout**
- Solution: Login again, check session storage directory permissions

**Issue: Upload failures**
- Solution: Check file size limits, verify GD extension is enabled, check upload directory permissions

### Error Logs

Check PHP error logs for detailed error information:
- Apache: `/var/log/apache2/error.log`
- Nginx: `/var/log/nginx/error.log`
- Custom: Check your server configuration

### Getting Help

For technical support:
- Email: deepanshurao938@gmail.com
- Include error messages and steps to reproduce the issue

## Contact and Support

### Developer Contact

For technical issues, feature requests, or questions:
- **Email**: deepanshurao938@gmail.com

### Additional Resources

- **Admin Panel**: `/admin/login.php`
- **Contact Form**: `/contact.php`
- **Profile Page**: `/profile.php`

## Maintenance Schedule

### Daily
- Monitor error logs
- Check contact form submissions

### Weekly
- Review and approve new content
- Check backup files

### Monthly
- Review and update security settings
- Clean up old backups
- Review user access logs

### Quarterly
- Update PHP and dependencies
- Review and update content
- Test backup recovery process

## Version Information

- **Current PHP Version**: 8.2.12
- **Minimum PHP Version**: 8.0
- **Database**: SQLite 3
- **Last Updated**: June 2026

---

**Note**: This manual is for site administrators and clients. For technical development documentation, please contact the developer.

**Author**: Deepanshu Khairwal
**THE SETTING SUN Web Solutions**