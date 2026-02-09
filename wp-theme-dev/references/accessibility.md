# Accessibility Reference — WCAG 2.1 AA + WordPress Accessibility-Ready

Sources:
- https://make.wordpress.org/themes/handbook/review/accessibility/required/
- https://www.w3.org/WAI/WCAG21/quickref/?levels=aaa#principle1
- https://make.wordpress.org/themes/handbook/review/required/#3-accessibility

## Skip Links

**Required for ALL themes** (not just accessibility-ready).

Must be the first focusable element on the page. Visible on keyboard focus. Targets `#primary` or `<main>`.

### Block Theme
Skip links are added automatically to the `<main>` tag in block themes. No action needed unless
the theme uses a custom header template part that adds content before main.

### Classic Theme
```php
<!-- Immediately after <body> and wp_body_open() -->
<a class="skip-link screen-reader-text" href="#primary">
    <?php esc_html_e( 'Skip to content', '{theme-slug}' ); ?>
</a>
```

```css
.skip-link:focus {
    background-color: #f1f1f1;
    border-radius: 3px;
    box-shadow: 0 0 2px 2px rgba(0, 0, 0, 0.6);
    clip: auto !important;
    clip-path: none;
    color: #21759b;
    display: block;
    font-size: 0.875rem;
    font-weight: 700;
    height: auto;
    left: 5px;
    line-height: normal;
    padding: 15px 23px 14px;
    text-decoration: none;
    top: 5px;
    width: auto;
    z-index: 100000;
}
```

## Keyboard Navigation

**Required for ALL themes.**

All interactive elements must be:
- Reachable via Tab key
- Operable via Enter/Space keys
- Visible focus indicator when focused (not just `:hover`)
- Logical tab order matching visual layout
- Functional at all viewport sizes (including responsive/mobile views)

### Focus Styles — MANDATORY
```css
/* Visible focus for all interactive elements */
a:focus,
button:focus,
input:focus,
select:focus,
textarea:focus,
[tabindex]:focus {
    outline: 2px solid var(--wp--preset--color--accent, #0073aa);
    outline-offset: 2px;
}

/* Remove default outline only if replacing with custom visible focus */
a:focus-visible,
button:focus-visible {
    outline: 2px solid var(--wp--preset--color--accent, #0073aa);
    outline-offset: 2px;
}
```

### Dropdown Menus
- NEVER hide with `display: none` triggered only by `:hover`
- Show on both `:hover` AND `:focus-within`
- Allow keyboard navigation through all menu items
- Use `aria-expanded` to indicate open/closed state

```css
/* Accessible dropdown */
.sub-menu {
    position: absolute;
    clip: rect(1px, 1px, 1px, 1px);
    clip-path: inset(50%);
}

.menu-item:hover > .sub-menu,
.menu-item:focus-within > .sub-menu {
    clip: auto;
    clip-path: none;
}
```

### Mobile Menu Toggle
```html
<button class="menu-toggle"
    type="button"
    aria-controls="primary-menu"
    aria-expanded="false">
    <span class="screen-reader-text">Menu</span>
</button>
```

```javascript
const toggle = document.querySelector('.menu-toggle');
const menu = document.getElementById('primary-menu');

toggle.addEventListener('click', () => {
    const expanded = toggle.getAttribute('aria-expanded') === 'true';
    toggle.setAttribute('aria-expanded', String(!expanded));
    menu.classList.toggle('toggled');
});
```

## Semantic HTML & ARIA Landmarks

Use HTML5 semantic elements. Add ARIA roles for older browser support.

| HTML Element | ARIA Role | Usage |
|---|---|---|
| `<header>` | `role="banner"` | Site header (once per page) |
| `<main>` | `role="main"` | Main content area (once per page) |
| `<nav>` | `role="navigation"` | Navigation sections |
| `<aside>` | `role="complementary"` | Sidebars |
| `<footer>` | `role="contentinfo"` | Site footer (once per page) |
| `<form>` (search) | `role="search"` | Search forms |

### Unique Labels for Duplicate Landmarks
When using multiple `<nav>` elements, each MUST have a unique `aria-label`:

```html
<nav aria-label="<?php esc_attr_e( 'Primary', '{theme-slug}' ); ?>">
<nav aria-label="<?php esc_attr_e( 'Footer', '{theme-slug}' ); ?>">
<nav aria-label="<?php esc_attr_e( 'Social Links', '{theme-slug}' ); ?>">
```

### Navigation widgets support (Required)
```php
add_theme_support( 'html5', array( 'navigation-widgets' ) );
```

## Heading Hierarchy

- Every page MUST have exactly one `<h1>`
- Headings must NOT skip levels (h1 → h3 is invalid)
- On single posts/pages: the post title is `<h1>`
- On archive/home: the page/archive title is `<h1>`, post titles are `<h2>`
- Widget titles should be `<h2>` in sidebars
- Never use heading tags for visual styling only

### Classic Theme Pattern
```php
<?php if ( is_singular() ) : ?>
    <h1 class="entry-title"><?php the_title(); ?></h1>
<?php else : ?>
    <h2 class="entry-title">
        <a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>
    </h2>
<?php endif; ?>
```

## Color Contrast

WCAG 2.1 Level AA minimum ratios:

| Element Type | Minimum Ratio |
|---|---|
| Normal text (<18px bold, <24px regular) | **4.5:1** |
| Large text (≥18px bold, ≥24px regular) | **3:1** |
| UI components & focus indicators | **3:1** |
| Decorative/disabled elements | No requirement |

### How to Verify
- Use browser DevTools contrast checker
- WebAIM Contrast Checker: https://webaim.org/resources/contrastchecker/
- Lighthouse accessibility audit

### Implementation
- Define accessible color pairs in theme.json palette
- Test ALL combinations of background + text color that users can select
- Ensure focus indicators have sufficient contrast against both light and dark backgrounds

## Content Links

**Required for ALL themes.**

Links within body text (post content, comment content, text widgets) MUST be underlined.
The underline is the ONLY accepted method. Bold, italic, or color-only is NOT acceptable.

### Block Theme (theme.json)
```json
{
    "styles": {
        "elements": {
            "link": {
                "typography": {
                    "textDecoration": "underline"
                }
            }
        }
    }
}
```

### Classic Theme (CSS)
```css
.entry-content a,
.comment-content a,
.widget_text a {
    text-decoration: underline;
}
```

Links in navigation menus, post meta, and widget lists do NOT need underlines.

## Forms

- ALL form inputs MUST have explicit `<label>` elements
- Placeholder text is NOT a valid replacement for labels
- Labels can be visually hidden with `.screen-reader-text` but must be present in DOM
- Error messages must be associated with their fields via `aria-describedby`
- Form submissions must have visible submit buttons (can be screen-reader-text hidden)

### Search Form
```php
<form role="search" method="get" class="search-form"
      action="<?php echo esc_url( home_url( '/' ) ); ?>">
    <label for="search-field">
        <span class="screen-reader-text">
            <?php echo esc_html_x( 'Search for:', 'label', '{theme-slug}' ); ?>
        </span>
    </label>
    <input type="search" id="search-field" class="search-field"
           placeholder="<?php echo esc_attr_x( 'Search &hellip;', 'placeholder', '{theme-slug}' ); ?>"
           value="<?php echo get_search_query(); ?>" name="s" />
    <button type="submit" class="search-submit">
        <span class="screen-reader-text">
            <?php echo esc_html_x( 'Search', 'submit button', '{theme-slug}' ); ?>
        </span>
    </button>
</form>
```

## Images

- ALL `<img>` tags MUST have `alt` attributes
- Informative images: descriptive alt text
- Decorative images: `alt=""`
- Post thumbnails should use the post title as alt fallback
- Never use images of text as a replacement for actual text

```php
the_post_thumbnail( 'large', array(
    'alt' => the_title_attribute( array( 'echo' => false ) ),
) );
```

## Screen Reader Text

The `.screen-reader-text` class hides text visually but keeps it accessible:

```css
.screen-reader-text {
    border: 0;
    clip: rect(1px, 1px, 1px, 1px);
    clip-path: inset(50%);
    height: 1px;
    margin: -1px;
    overflow: hidden;
    padding: 0;
    position: absolute !important;
    width: 1px;
    word-wrap: normal !important;
}
```

Use for:
- Skip links (before focus)
- Labels on icon-only buttons
- Post titles in "Read more" links
- Section headings that would be redundant visually
- Navigation labels

## Repetitive Link Text

"Read more" links MUST include the post title for screen reader context:

```php
the_content(
    sprintf(
        '%1$s<span class="screen-reader-text"> "%2$s"</span>',
        esc_html__( 'Continue reading', '{theme-slug}' ),
        esc_html( get_the_title() )
    )
);
```

Bare URLs must never be used as link text. Link text must make sense out of context.

## Media

- Embedded audio/video MUST have controls
- NO auto-play for audio or video
- Provide text alternatives where possible
- Use WordPress embed blocks which handle controls automatically

## Controls

All interactive elements MUST use proper HTML elements:
- Clickable actions → `<button>` or `<a>`
- NEVER use `<span>` or `<div>` as clickable elements
- All controls must have accessible names (visible text, `aria-label`, or screen-reader-text)

**Fails:** `<span class="menu-toggle"><span class="dashicons dashicons-menu"></span></span>`
**Passes:** `<button type="button" class="menu-toggle"><span class="screen-reader-text">Menu</span></button>`

## Reduced Motion

Respect user's motion preferences:

```css
@media (prefers-reduced-motion: reduce) {
    *,
    *::before,
    *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
        scroll-behavior: auto !important;
    }
}
```

## Testing Checklist

1. **Keyboard:** Tab through entire page — all elements reachable? Focus visible?
2. **Skip link:** First Tab press shows skip link? Activating it moves to content?
3. **Screen reader:** Test with NVDA (Windows) or VoiceOver (Mac) — logical reading order?
4. **Headings:** Use HeadingsMap extension — proper hierarchy, no skips?
5. **Contrast:** Run Lighthouse audit — all ratios pass?
6. **Zoom:** Zoom to 200% — content still usable, no horizontal scroll?
7. **Links:** Content links underlined? "Read more" includes title?
8. **Forms:** All inputs labeled? Errors perceivable?
9. **Images:** All images have alt text? Decorative images have `alt=""`?
10. **Landmarks:** Use Landmark Navigation extension — all content inside a landmark?
