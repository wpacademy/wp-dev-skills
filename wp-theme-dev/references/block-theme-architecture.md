# Block Theme Architecture Reference

## Block Theme Structure

```
theme-slug/
├── parts/                          ← Template parts (header, footer, sidebar)
│   ├── header.html
│   ├── footer.html
│   └── sidebar.html
├── patterns/                       ← Block patterns (auto-registered)
│   ├── hero.php
│   ├── cta.php
│   └── featured-posts.php
├── styles/                         ← Style variations (JSON)
│   ├── dark.json
│   └── warm.json
├── templates/                      ← Page templates (HTML)
│   ├── index.html                  ← REQUIRED — fallback template
│   ├── 404.html
│   ├── archive.html
│   ├── home.html
│   ├── page.html
│   ├── single.html
│   └── search.html
├── assets/
│   ├── css/
│   │   └── custom-blocks.css
│   ├── js/
│   │   └── navigation.js
│   ├── images/
│   └── fonts/
├── inc/                            ← Additional PHP (loaded from functions.php)
│   └── block-styles.php
├── functions.php                   ← Theme setup, enqueues, pattern registration
├── style.css                       ← REQUIRED — theme metadata + custom CSS
├── theme.json                      ← Global settings & styles
├── readme.txt                      ← WordPress.org readme
├── screenshot.png                  ← 1200×900 theme preview
└── LICENSE
```

## Required Files

Only two files are strictly required by WordPress:
- `style.css` — Theme metadata header
- `templates/index.html` — Fallback template

## style.css Header (All Required Fields)

```css
/*
Theme Name:       {Theme Name}
Theme URI:        {Theme URI}
Author:           {Author Name}
Author URI:       {Author URI}
Description:      {Short description of the theme.}
Version:          1.0.0
Requires at least: 6.0
Tested up to:     6.7
Requires PHP:     7.4
License:          GNU General Public License v2 or later
License URI:      https://www.gnu.org/licenses/gpl-2.0.html
Text Domain:      {theme-slug}
Tags:             block-patterns, full-site-editing, wide-blocks
*/
```

## theme.json — Global Settings & Styles

```json
{
    "$schema": "https://schemas.wp.org/trunk/theme.json",
    "version": 3,
    "settings": {
        "appearanceTools": true,
        "color": {
            "defaultDuotone": false,
            "defaultGradients": false,
            "defaultPalette": false,
            "palette": [
                {
                    "color": "#1a1a2e",
                    "name": "Primary",
                    "slug": "primary"
                },
                {
                    "color": "#e94560",
                    "name": "Accent",
                    "slug": "accent"
                },
                {
                    "color": "#ffffff",
                    "name": "Base",
                    "slug": "base"
                },
                {
                    "color": "#1a1a2e",
                    "name": "Contrast",
                    "slug": "contrast"
                }
            ]
        },
        "layout": {
            "contentSize": "720px",
            "wideSize": "1200px"
        },
        "spacing": {
            "spacingScale": {
                "steps": 0
            },
            "spacingSizes": [
                { "name": "Small", "size": "clamp(1rem, 2vw, 1.5rem)", "slug": "30" },
                { "name": "Medium", "size": "clamp(1.5rem, 3vw, 2.5rem)", "slug": "40" },
                { "name": "Large", "size": "clamp(2rem, 4vw, 4rem)", "slug": "50" },
                { "name": "X-Large", "size": "clamp(3rem, 6vw, 6rem)", "slug": "60" }
            ],
            "units": [ "%", "px", "em", "rem", "vh", "vw" ]
        },
        "typography": {
            "defaultFontSizes": false,
            "fluid": true,
            "fontFamilies": [
                {
                    "fontFamily": "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif",
                    "name": "System Sans",
                    "slug": "system-sans"
                },
                {
                    "fontFamily": "'Georgia', 'Times New Roman', serif",
                    "name": "System Serif",
                    "slug": "system-serif"
                }
            ],
            "fontSizes": [
                { "fluid": { "min": "0.875rem", "max": "1rem" }, "name": "Small", "size": "1rem", "slug": "small" },
                { "fluid": { "min": "1rem", "max": "1.125rem" }, "name": "Medium", "size": "1.125rem", "slug": "medium" },
                { "fluid": { "min": "1.125rem", "max": "1.5rem" }, "name": "Large", "size": "1.5rem", "slug": "large" },
                { "fluid": { "min": "1.5rem", "max": "2.25rem" }, "name": "X-Large", "size": "2.25rem", "slug": "x-large" },
                { "fluid": { "min": "2rem", "max": "3.5rem" }, "name": "XX-Large", "size": "3.5rem", "slug": "xx-large" }
            ]
        },
        "useRootPaddingAwareAlignments": true
    },
    "styles": {
        "color": {
            "background": "var(--wp--preset--color--base)",
            "text": "var(--wp--preset--color--contrast)"
        },
        "elements": {
            "button": {
                "border": { "radius": "4px" },
                "color": {
                    "background": "var(--wp--preset--color--accent)",
                    "text": "var(--wp--preset--color--base)"
                },
                ":hover": {
                    "color": {
                        "background": "var(--wp--preset--color--contrast)",
                        "text": "var(--wp--preset--color--base)"
                    }
                },
                ":focus": {
                    "color": {
                        "background": "var(--wp--preset--color--contrast)",
                        "text": "var(--wp--preset--color--base)"
                    },
                    "outline": {
                        "color": "var(--wp--preset--color--accent)",
                        "offset": "2px",
                        "style": "solid",
                        "width": "2px"
                    }
                }
            },
            "link": {
                "color": { "text": "var(--wp--preset--color--accent)" },
                "typography": { "textDecoration": "underline" },
                ":hover": {
                    "color": { "text": "var(--wp--preset--color--contrast)" }
                },
                ":focus": {
                    "outline": {
                        "color": "var(--wp--preset--color--accent)",
                        "offset": "2px",
                        "style": "solid",
                        "width": "2px"
                    }
                }
            }
        },
        "spacing": {
            "blockGap": "1.5rem",
            "padding": {
                "left": "var(--wp--preset--spacing--40)",
                "right": "var(--wp--preset--spacing--40)"
            }
        },
        "typography": {
            "fontFamily": "var(--wp--preset--font-family--system-sans)",
            "fontSize": "var(--wp--preset--font-size--medium)",
            "lineHeight": "1.6"
        }
    },
    "templateParts": [
        { "area": "header", "name": "header", "title": "Header" },
        { "area": "footer", "name": "footer", "title": "Footer" }
    ],
    "customTemplates": [
        { "name": "page-no-title", "postTypes": ["page"], "title": "Page (No Title)" },
        { "name": "page-wide", "postTypes": ["page"], "title": "Page (Wide)" }
    ]
}
```

### Key theme.json Principles
- Set `defaultPalette: false`, `defaultGradients: false`, `defaultFontSizes: false` to control the exact options available to users
- Use `fluid` typography for responsive font sizes without media queries
- Use `clamp()` in spacing for fluid responsive spacing
- Define ALL colors in the palette — users choose from these in the editor
- Set `useRootPaddingAwareAlignments: true` for proper full-width alignment handling
- Style elements (links, buttons, headings) globally via `styles.elements`
- **Links MUST have `textDecoration: underline`** in content contexts for accessibility

## Template Parts

### parts/header.html
```html
<!-- wp:group {"tagName":"header","layout":{"type":"constrained"}} -->
<header class="wp-block-group">
    <!-- wp:group {"layout":{"type":"flex","justifyContent":"space-between","flexWrap":"wrap"}} -->
    <div class="wp-block-group">
        <!-- wp:site-title {"level":1} /-->
        <!-- wp:navigation {"overlayMenu":"mobile","layout":{"type":"flex"}} /-->
    </div>
    <!-- /wp:group -->
</header>
<!-- /wp:group -->
```

### parts/footer.html
```html
<!-- wp:group {"tagName":"footer","style":{"spacing":{"padding":{"top":"var:preset|spacing|50","bottom":"var:preset|spacing|50"}}},"layout":{"type":"constrained"}} -->
<footer class="wp-block-group">
    <!-- wp:group {"layout":{"type":"flex","justifyContent":"space-between","flexWrap":"wrap"}} -->
    <div class="wp-block-group">
        <!-- wp:paragraph {"fontSize":"small"} -->
        <p class="has-small-font-size">
            © <!-- wp:post-date {"format":"Y","isLink":false} /--> {Theme Name}
        </p>
        <!-- /wp:paragraph -->
        <!-- wp:paragraph {"fontSize":"small"} -->
        <p class="has-small-font-size">
            Powered by <a href="https://wordpress.org">WordPress</a>
        </p>
        <!-- /wp:paragraph -->
    </div>
    <!-- /wp:group -->
</footer>
<!-- /wp:group -->
```

## Templates

### templates/index.html (Required Fallback)
```html
<!-- wp:template-part {"slug":"header","area":"header"} /-->

<!-- wp:group {"tagName":"main","layout":{"type":"constrained"}} -->
<main class="wp-block-group">
    <!-- wp:query-title {"type":"archive"} /-->
    <!-- wp:query {"queryId":1,"query":{"perPage":10,"pages":0,"offset":0,"postType":"post","order":"desc","orderBy":"date","inherit":true}} -->
    <div class="wp-block-query">
        <!-- wp:post-template -->
            <!-- wp:post-title {"isLink":true} /-->
            <!-- wp:post-featured-image {"isLink":true} /-->
            <!-- wp:post-excerpt {"moreText":"Continue reading","excerptLength":40} /-->
            <!-- wp:post-date /-->
        <!-- /wp:post-template -->
        <!-- wp:query-pagination {"layout":{"type":"flex","justifyContent":"space-between"}} -->
            <!-- wp:query-pagination-previous /-->
            <!-- wp:query-pagination-numbers /-->
            <!-- wp:query-pagination-next /-->
        <!-- /wp:query-pagination -->
        <!-- wp:query-no-results -->
            <!-- wp:paragraph -->
            <p>No posts found.</p>
            <!-- /wp:paragraph -->
        <!-- /wp:query-no-results -->
    </div>
    <!-- /wp:query -->
</main>
<!-- /wp:group -->

<!-- wp:template-part {"slug":"footer","area":"footer"} /-->
```

### templates/single.html
```html
<!-- wp:template-part {"slug":"header","area":"header"} /-->

<!-- wp:group {"tagName":"main","layout":{"type":"constrained"}} -->
<main class="wp-block-group">
    <!-- wp:post-title {"level":1} /-->
    <!-- wp:group {"layout":{"type":"flex","flexWrap":"wrap"},"fontSize":"small"} -->
    <div class="wp-block-group">
        <!-- wp:post-date /-->
        <!-- wp:post-author-name {"isLink":true} /-->
        <!-- wp:post-terms {"term":"category"} /-->
    </div>
    <!-- /wp:group -->
    <!-- wp:post-featured-image {"align":"wide"} /-->
    <!-- wp:post-content {"layout":{"type":"constrained"}} /-->
    <!-- wp:post-terms {"term":"post_tag","separator":"  "} /-->
    <!-- wp:post-navigation-link {"type":"previous"} /-->
    <!-- wp:post-navigation-link /-->
    <!-- wp:comments /-->
</main>
<!-- /wp:group -->

<!-- wp:template-part {"slug":"footer","area":"footer"} /-->
```

### templates/404.html
```html
<!-- wp:template-part {"slug":"header","area":"header"} /-->

<!-- wp:group {"tagName":"main","layout":{"type":"constrained"}} -->
<main class="wp-block-group">
    <!-- wp:heading {"level":1} -->
    <h1>Page Not Found</h1>
    <!-- /wp:heading -->
    <!-- wp:paragraph -->
    <p>The page you are looking for does not exist. Try searching for it:</p>
    <!-- /wp:paragraph -->
    <!-- wp:search {"label":"Search","showLabel":false,"buttonText":"Search"} /-->
</main>
<!-- /wp:group -->

<!-- wp:template-part {"slug":"footer","area":"footer"} /-->
```

## Block Patterns

Patterns are PHP files in the `patterns/` directory and are auto-registered by WordPress.

### Pattern File Header
```php
<?php
/**
 * Title: Hero Section
 * Slug: {theme-slug}/hero
 * Categories: featured, banner
 * Keywords: hero, banner, call to action
 * Description: A full-width hero section with heading, text, and buttons.
 * Viewport Width: 1200
 */
?>
<!-- wp:cover {"dimRatio":50,"overlayColor":"contrast","isUserOverlayColor":true,"minHeight":500,"align":"full","layout":{"type":"constrained"}} -->
<div class="wp-block-cover alignfull" style="min-height:500px">
    <span aria-hidden="true" class="wp-block-cover__background has-contrast-background-color has-background-dim"></span>
    <div class="wp-block-cover__inner-container">
        <!-- wp:heading {"textAlign":"center","level":1,"textColor":"base"} -->
        <h1 class="wp-block-heading has-text-align-center has-base-color has-text-color">
            <?php echo esc_html__( 'Welcome to Our Site', '{theme-slug}' ); ?>
        </h1>
        <!-- /wp:heading -->
        <!-- wp:paragraph {"align":"center","textColor":"base"} -->
        <p class="has-text-align-center has-base-color has-text-color">
            <?php echo esc_html__( 'A brief description of what this site is about.', '{theme-slug}' ); ?>
        </p>
        <!-- /wp:paragraph -->
        <!-- wp:buttons {"layout":{"type":"flex","justifyContent":"center"}} -->
        <div class="wp-block-buttons">
            <!-- wp:button -->
            <div class="wp-block-button">
                <a class="wp-block-button__link wp-element-button">
                    <?php echo esc_html__( 'Get Started', '{theme-slug}' ); ?>
                </a>
            </div>
            <!-- /wp:button -->
        </div>
        <!-- /wp:buttons -->
    </div>
</div>
<!-- /wp:cover -->
```

### Pattern Best Practices
- Use `esc_html__()` for all translatable text within patterns
- Register a custom pattern category in functions.php for the theme's patterns
- Group related patterns with consistent naming: `{theme-slug}/hero`, `{theme-slug}/cta`, etc.
- Set appropriate `Viewport Width` for the pattern preview
- Always include `Keywords` for discoverability

## Style Variations

Style variations are JSON files in the `styles/` directory that override theme.json settings:

### styles/dark.json
```json
{
    "$schema": "https://schemas.wp.org/trunk/theme.json",
    "version": 3,
    "title": "Dark",
    "settings": {
        "color": {
            "palette": [
                { "color": "#0d1117", "name": "Base", "slug": "base" },
                { "color": "#e6edf3", "name": "Contrast", "slug": "contrast" },
                { "color": "#58a6ff", "name": "Accent", "slug": "accent" },
                { "color": "#161b22", "name": "Primary", "slug": "primary" }
            ]
        }
    },
    "styles": {
        "color": {
            "background": "var(--wp--preset--color--base)",
            "text": "var(--wp--preset--color--contrast)"
        }
    }
}
```

## functions.php Pattern

```php
<?php
/**
 * {Theme Name} functions and definitions.
 *
 * @package {Theme_Name}
 * @since   1.0.0
 */

if ( ! defined( 'ABSPATH' ) ) {
    exit;
}

define( '{THEME_PREFIX}_VERSION', '1.0.0' );

/**
 * Theme setup.
 *
 * @since 1.0.0
 */
function {prefix}_setup() {
    // Make theme available for translation.
    load_theme_textdomain( '{theme-slug}', get_template_directory() . '/languages' );

    // Add support for block styles.
    add_theme_support( 'wp-block-styles' );

    // Enqueue editor styles.
    add_editor_style( 'style.css' );

    // HTML5 support.
    add_theme_support( 'html5', array(
        'comment-form',
        'comment-list',
        'gallery',
        'caption',
        'style',
        'script',
        'navigation-widgets',
    ) );

    // Responsive embeds.
    add_theme_support( 'responsive-embeds' );
}
add_action( 'after_setup_theme', '{prefix}_setup' );

/**
 * Enqueue styles.
 *
 * @since 1.0.0
 */
function {prefix}_enqueue_styles() {
    wp_enqueue_style(
        '{theme-slug}-style',
        get_stylesheet_uri(),
        array(),
        {THEME_PREFIX}_VERSION
    );
}
add_action( 'wp_enqueue_scripts', '{prefix}_enqueue_styles' );

/**
 * Register block pattern categories.
 *
 * @since 1.0.0
 */
function {prefix}_register_pattern_categories() {
    register_block_pattern_category(
        '{theme-slug}',
        array(
            'label' => __( '{Theme Name}', '{theme-slug}' ),
        )
    );
}
add_action( 'init', '{prefix}_register_pattern_categories' );
```

## Enqueueing Assets Properly

```php
// Frontend styles
function {prefix}_enqueue_styles() {
    wp_enqueue_style(
        '{theme-slug}-style',
        get_stylesheet_uri(),
        array(),
        {THEME_PREFIX}_VERSION
    );

    // Additional CSS
    wp_enqueue_style(
        '{theme-slug}-custom',
        get_template_directory_uri() . '/assets/css/custom.css',
        array( '{theme-slug}-style' ),
        {THEME_PREFIX}_VERSION
    );
}

// Frontend scripts (with defer)
function {prefix}_enqueue_scripts() {
    wp_enqueue_script(
        '{theme-slug}-navigation',
        get_template_directory_uri() . '/assets/js/navigation.js',
        array(),
        {THEME_PREFIX}_VERSION,
        array( 'strategy' => 'defer' )
    );
}

// Editor styles
function {prefix}_enqueue_editor_styles() {
    add_editor_style( array(
        'style.css',
        'assets/css/editor.css',
    ) );
}
```

## Child Theme Structure (Block)

```
child-theme-slug/
├── parts/          ← Override parent parts
├── patterns/       ← Add new or override patterns
├── styles/         ← Add style variations
├── templates/      ← Override parent templates
├── style.css       ← With "Template: parent-slug" header
└── theme.json      ← Overrides merged with parent
```

### Child theme style.css
```css
/*
Theme Name:       {Child Theme Name}
Template:         {parent-theme-slug}
Version:          1.0.0
License:          GNU General Public License v2 or later
License URI:      https://www.gnu.org/licenses/gpl-2.0.html
Text Domain:      {child-theme-slug}
*/
```
