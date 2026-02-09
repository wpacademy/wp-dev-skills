# WordPress.org Theme Review Requirements (Condensed)

Sources:
- https://make.wordpress.org/themes/handbook/review/required/
- https://developer.wordpress.org/themes/releasing-your-theme/testing/
- https://developer.wordpress.org/themes/releasing-your-theme/theme-review-guidelines/

Themes with 3+ distinct issues may be closed as not-approved.

## 1. Licensing & Copyright

- GPL v2 or later for ALL code, data, images, fonts — everything in the zip
- Third-party libraries must be GPL-compatible
- Include copyright info for the theme itself
- Include license, copyright, and source for ALL bundled resources (fonts, images, scripts)
- List all resources in one file (readme.txt recommended)
- All code and design must be original or legally yours — no cloning
- Frontend copyright must show the USER's copyright, not the theme author's

## 2. Privacy

- Any tracking/data collection must be disabled by default, opt-in only
- Document data collection in readme.txt with a privacy policy

## 3. Accessibility

**Required for ALL themes (not just accessibility-ready):**
- Skip links: first focusable element, visible on focus, targets main content
- Keyboard navigation: visible focus on all interactive elements, all controls reachable
- Content links must be underlined (bold/italic/color-only is NOT acceptable)

**If tagged `accessibility-ready`:** must also meet full accessibility audit requirements
(see `references/accessibility.md`)

## 4. Code

- No PHP or JavaScript errors, warnings, or notices
- Validate and sanitize all untrusted data before database
- Escape all untrusted data before output
- No deprecated functions or constants
- Unique prefix for EVERYTHING (4+ character minimum): functions, options, globals, constants, post meta, enqueue handles, image sizes, pattern categories, etc.
- Exception: third-party asset handles and menu location/sidebar IDs don't need prefix

## 5. Functionality and Features

**Do NOT:**
- Place WordPress features behind a paywall
- Remove, hide, or block the admin bar
- Redirect on theme activation or modify activation process
- Remove non-presentational hooks (wp_generator, feed_links, rest_output_link_wp_head, etc.)

**Do NOT include:**
- Custom post types
- Custom blocks (register in a companion plugin instead)
- Custom roles
- Custom user contact methods
- Custom mime types
- Shortcodes
- Any functionality not related to design and presentation

**Admin notices:**
- Use the `admin_notices` API
- All notices must be dismissible
- Follow core UI design for notices

## 6. Plugins

- Only recommend plugins hosted on WordPress.org
- Plugins installed by user action only, never automatically
- Do not include zip files, download plugins, or include plugin functionality
- Plugin territory examples: analytics, SEO, contact forms, social buttons, caching, session tampering

## 7. Naming, Spelling, and Trademarks

- Theme names must NOT use: "WordPress", "Theme", "Twenty*"
- Spell "WordPress" correctly (capital W and P, one word)
- No trademark violations

## 8. Language & Internationalization

- All text strings must be translatable using gettext
- Use theme slug as text domain, added to style.css
- Maximum two text domains (one for theme, one for allowed framework like TGMPA, Kirki)

## 9. Files

**Required files:**
- style.css with all required headers
- readme.txt with required format
- templates/index.html (block) or index.php (classic)
- screenshot.png (1200×900)

**Required style.css headers:**
```
Theme Name, Author, Description, Version, Requires at least,
Tested up to, Requires PHP, License, License URI, Text Domain
```

**readme.txt must include:**
- Contributors (WordPress.org username)
- Requires at least, Tested up to, Requires PHP
- License info
- Description, Changelog
- Resources section listing ALL bundled assets with license and source

**Not allowed files:** thumbs.db, desktop.ini, .sql, .sh, .zip, hidden files/folders, favicon.ico, error_log, php.ini, IDE project files

**Stylesheets/scripts:**
- When using minified files, include originals too
- Use WordPress bundled libraries (jQuery, etc.)

**Remote resources:**
- No CDNs or remote resources without user consent
- Google Fonts is the only exception
- Prefer locally hosted fonts for GDPR compliance

**Screenshot:**
- Must represent actual theme output
- Must not be an advertisement
- 1200×900 pixels recommended

## 10. Classic Themes (Additional)

- Must use `wp_head()` and `wp_footer()` hooks
- Must use `wp_body_open()` immediately after `<body>`
- Must use `body_class()` on the body tag
- Must use `post_class()` on article containers
- Must use `language_attributes()` on the html tag
- Must use `bloginfo( 'charset' )` for charset
- Must use `wp_enqueue_*` for all styles and scripts (no hardcoded tags)
- Must use `add_theme_support( 'title-tag' )` instead of `<title>` tags
- The theme header must call `wp_head()` and the footer `wp_footer()`

## 11. Block Themes (Additional)

- Must include `templates/index.html` as fallback template
- Must include `theme.json` for settings configuration
- Patterns must use proper header format with Title, Slug, Categories
- Template parts must be registered in theme.json `templateParts` array

## 12. Theme Settings Pages and Onboarding

- Settings pages should use the Customizer API (classic) or theme.json (block)
- No custom top-level admin menu pages for theme settings
- Onboarding/setup wizards are discouraged
- No forced redirect to settings on activation

## 13. Selling, Credits, Links, and Spam

- One "upgrade/pro" link is allowed in the Customizer
- One admin notice for upgrade is allowed (must be dismissible, on theme pages only)
- Credit links must be optional, not required for theme to function
- No affiliate links in the theme
- No links to stand-alone paid add-on sites
- No keyword stuffing in readme or style.css

## 14. Theme Author and Upload Restrictions

- Each theme must have a unique design — no duplicate themes with trivial changes
- Authors may be restricted if they submit many low-quality themes
- Theme must be complete at submission
- Updates must go through the review queue

## Testing Before Submission

1. Enable `WP_DEBUG`, `WP_DEBUG_LOG`, `WP_DEBUG_DISPLAY` — fix ALL errors
2. Run Theme Check plugin
3. Import Theme Unit Test data (https://github.com/WordPress/theme-test-data)
4. Test with Theme Sniffer plugin
5. Test keyboard navigation on all pages
6. Run Lighthouse accessibility audit
7. Verify all responsive breakpoints
8. Test RTL layout with RTL Tester plugin
9. Test with multiple browsers
10. Verify all strings are translatable
