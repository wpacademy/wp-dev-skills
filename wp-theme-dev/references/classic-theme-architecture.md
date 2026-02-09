# Classic Theme Architecture Reference

## Classic Theme Structure

```
theme-slug/
├── assets/
│   ├── css/
│   │   └── custom.css
│   ├── js/
│   │   └── navigation.js
│   ├── images/
│   └── fonts/
├── inc/
│   ├── customizer.php
│   ├── template-functions.php
│   └── template-tags.php
├── template-parts/
│   ├── content/
│   │   ├── content.php
│   │   ├── content-page.php
│   │   ├── content-search.php
│   │   └── content-none.php
│   ├── header/
│   │   └── site-branding.php
│   └── footer/
│       └── footer-widgets.php
├── languages/
│   └── {theme-slug}.pot
├── 404.php
├── archive.php
├── comments.php
├── footer.php
├── functions.php
├── header.php
├── index.php                       ← REQUIRED — fallback template
├── page.php
├── search.php
├── searchform.php
├── sidebar.php
├── single.php
├── style.css                       ← REQUIRED — theme metadata
├── readme.txt
├── screenshot.png
└── LICENSE
```

## Template Hierarchy (Simplified)

WordPress selects templates in order of specificity:

```
Single Post:   single-{post-type}-{slug}.php → single-{post-type}.php → single.php → singular.php → index.php
Page:          page-{slug}.php → page-{id}.php → page.php → singular.php → index.php
Category:      category-{slug}.php → category-{id}.php → category.php → archive.php → index.php
Tag:           tag-{slug}.php → tag-{id}.php → tag.php → archive.php → index.php
Author:        author-{nicename}.php → author-{id}.php → author.php → archive.php → index.php
Archive:       archive-{post-type}.php → archive.php → index.php
Search:        search.php → index.php
404:           404.php → index.php
Home (blog):   home.php → index.php
Front Page:    front-page.php → page.php / home.php → index.php
```

## header.php

```php
<?php
/**
 * The header template.
 *
 * @package {Theme_Name}
 * @since   1.0.0
 */

?>
<!doctype html>
<html <?php language_attributes(); ?>>
<head>
    <meta charset="<?php bloginfo( 'charset' ); ?>">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="profile" href="https://gmpg.org/xfn/11">
    <?php wp_head(); ?>
</head>

<body <?php body_class(); ?>>
<?php wp_body_open(); ?>

<a class="skip-link screen-reader-text" href="#primary">
    <?php esc_html_e( 'Skip to content', '{theme-slug}' ); ?>
</a>

<header id="masthead" class="site-header" role="banner">
    <div class="site-branding">
        <?php if ( has_custom_logo() ) : ?>
            <?php the_custom_logo(); ?>
        <?php endif; ?>

        <?php if ( is_front_page() && is_home() ) : ?>
            <h1 class="site-title">
                <a href="<?php echo esc_url( home_url( '/' ) ); ?>" rel="home">
                    <?php bloginfo( 'name' ); ?>
                </a>
            </h1>
        <?php else : ?>
            <p class="site-title">
                <a href="<?php echo esc_url( home_url( '/' ) ); ?>" rel="home">
                    <?php bloginfo( 'name' ); ?>
                </a>
            </p>
        <?php endif; ?>

        <?php
        $description = get_bloginfo( 'description', 'display' );
        if ( $description || is_customize_preview() ) :
            ?>
            <p class="site-description"><?php echo esc_html( $description ); ?></p>
        <?php endif; ?>
    </div>

    <nav id="site-navigation" class="main-navigation" role="navigation"
         aria-label="<?php esc_attr_e( 'Primary Menu', '{theme-slug}' ); ?>">
        <button class="menu-toggle" aria-controls="primary-menu" aria-expanded="false" type="button">
            <span class="screen-reader-text"><?php esc_html_e( 'Menu', '{theme-slug}' ); ?></span>
            <span class="menu-icon" aria-hidden="true"></span>
        </button>
        <?php
        wp_nav_menu( array(
            'theme_location' => 'primary',
            'menu_id'        => 'primary-menu',
            'container'      => false,
        ) );
        ?>
    </nav>
</header>
```

## footer.php

```php
<?php
/**
 * The footer template.
 *
 * @package {Theme_Name}
 * @since   1.0.0
 */

?>
    <footer id="colophon" class="site-footer" role="contentinfo">
        <?php if ( is_active_sidebar( '{theme-slug}-footer' ) ) : ?>
            <div class="footer-widgets">
                <?php dynamic_sidebar( '{theme-slug}-footer' ); ?>
            </div>
        <?php endif; ?>

        <div class="site-info">
            <p>
                &copy; <?php echo esc_html( date_i18n( 'Y' ) ); ?>
                <a href="<?php echo esc_url( home_url( '/' ) ); ?>">
                    <?php bloginfo( 'name' ); ?>
                </a>
            </p>
        </div>
    </footer>

<?php wp_footer(); ?>
</body>
</html>
```

## index.php (Fallback Template)

```php
<?php
/**
 * The main template file.
 *
 * @package {Theme_Name}
 * @since   1.0.0
 */

get_header();
?>

<main id="primary" class="site-main" role="main">
    <?php
    if ( have_posts() ) :

        if ( is_home() && ! is_front_page() ) :
            ?>
            <header class="page-header">
                <h1 class="page-title screen-reader-text">
                    <?php single_post_title(); ?>
                </h1>
            </header>
            <?php
        endif;

        while ( have_posts() ) :
            the_post();
            get_template_part( 'template-parts/content/content', get_post_type() );
        endwhile;

        the_posts_pagination( array(
            'prev_text' => sprintf(
                '<span class="screen-reader-text">%s</span>',
                esc_html__( 'Previous page', '{theme-slug}' )
            ),
            'next_text' => sprintf(
                '<span class="screen-reader-text">%s</span>',
                esc_html__( 'Next page', '{theme-slug}' )
            ),
        ) );

    else :
        get_template_part( 'template-parts/content/content', 'none' );
    endif;
    ?>
</main>

<?php
get_sidebar();
get_footer();
```

## functions.php (Classic Theme)

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
 */
function {prefix}_setup() {
    load_theme_textdomain( '{theme-slug}', get_template_directory() . '/languages' );

    add_theme_support( 'automatic-feed-links' );
    add_theme_support( 'title-tag' );
    add_theme_support( 'post-thumbnails' );
    add_theme_support( 'customize-selective-refresh-widgets' );
    add_theme_support( 'responsive-embeds' );
    add_theme_support( 'wp-block-styles' );
    add_theme_support( 'align-wide' );
    add_theme_support( 'editor-styles' );
    add_editor_style( 'assets/css/editor.css' );

    add_theme_support( 'custom-logo', array(
        'height'      => 250,
        'width'       => 250,
        'flex-height' => true,
        'flex-width'  => true,
    ) );

    add_theme_support( 'html5', array(
        'search-form',
        'comment-form',
        'comment-list',
        'gallery',
        'caption',
        'style',
        'script',
        'navigation-widgets',
    ) );

    register_nav_menus( array(
        'primary'   => esc_html__( 'Primary Menu', '{theme-slug}' ),
        'footer'    => esc_html__( 'Footer Menu', '{theme-slug}' ),
    ) );
}
add_action( 'after_setup_theme', '{prefix}_setup' );

/**
 * Register widget areas.
 */
function {prefix}_widgets_init() {
    register_sidebar( array(
        'name'          => esc_html__( 'Sidebar', '{theme-slug}' ),
        'id'            => '{theme-slug}-sidebar',
        'description'   => esc_html__( 'Add widgets here.', '{theme-slug}' ),
        'before_widget' => '<section id="%1$s" class="widget %2$s">',
        'after_widget'  => '</section>',
        'before_title'  => '<h2 class="widget-title">',
        'after_title'   => '</h2>',
    ) );

    register_sidebar( array(
        'name'          => esc_html__( 'Footer', '{theme-slug}' ),
        'id'            => '{theme-slug}-footer',
        'description'   => esc_html__( 'Add footer widgets here.', '{theme-slug}' ),
        'before_widget' => '<section id="%1$s" class="widget %2$s">',
        'after_widget'  => '</section>',
        'before_title'  => '<h2 class="widget-title">',
        'after_title'   => '</h2>',
    ) );
}
add_action( 'widgets_init', '{prefix}_widgets_init' );

/**
 * Enqueue styles and scripts.
 */
function {prefix}_scripts() {
    wp_enqueue_style(
        '{theme-slug}-style',
        get_stylesheet_uri(),
        array(),
        {THEME_PREFIX}_VERSION
    );

    wp_enqueue_script(
        '{theme-slug}-navigation',
        get_template_directory_uri() . '/assets/js/navigation.js',
        array(),
        {THEME_PREFIX}_VERSION,
        array( 'strategy' => 'defer' )
    );

    if ( is_singular() && comments_open() && get_option( 'thread_comments' ) ) {
        wp_enqueue_script( 'comment-reply' );
    }
}
add_action( 'wp_enqueue_scripts', '{prefix}_scripts' );

// Load additional files.
require get_template_directory() . '/inc/template-tags.php';
require get_template_directory() . '/inc/template-functions.php';
require get_template_directory() . '/inc/customizer.php';
```

## Template Part: content.php

```php
<?php
/**
 * Template part for displaying posts.
 *
 * @package {Theme_Name}
 * @since   1.0.0
 */

?>
<article id="post-<?php the_ID(); ?>" <?php post_class(); ?>>
    <header class="entry-header">
        <?php
        if ( is_singular() ) :
            the_title( '<h1 class="entry-title">', '</h1>' );
        else :
            the_title(
                '<h2 class="entry-title"><a href="' . esc_url( get_permalink() ) . '" rel="bookmark">',
                '</a></h2>'
            );
        endif;
        ?>

        <?php if ( 'post' === get_post_type() ) : ?>
            <div class="entry-meta">
                <?php
                printf(
                    '<time class="entry-date published" datetime="%1$s">%2$s</time>',
                    esc_attr( get_the_date( DATE_W3C ) ),
                    esc_html( get_the_date() )
                );
                ?>
                <span class="byline">
                    <?php
                    printf(
                        /* translators: %s: Author name. */
                        esc_html__( 'by %s', '{theme-slug}' ),
                        '<span class="author vcard"><a class="url fn n" href="' .
                        esc_url( get_author_posts_url( get_the_author_meta( 'ID' ) ) ) . '">' .
                        esc_html( get_the_author() ) . '</a></span>'
                    );
                    ?>
                </span>
            </div>
        <?php endif; ?>
    </header>

    <?php if ( has_post_thumbnail() && ! is_singular() ) : ?>
        <div class="post-thumbnail">
            <a href="<?php the_permalink(); ?>" aria-hidden="true" tabindex="-1">
                <?php the_post_thumbnail( 'large', array( 'alt' => the_title_attribute( 'echo=0' ) ) ); ?>
            </a>
        </div>
    <?php endif; ?>

    <div class="entry-content">
        <?php
        if ( is_singular() ) :
            the_content();

            wp_link_pages( array(
                'before' => '<div class="page-links">' . esc_html__( 'Pages:', '{theme-slug}' ),
                'after'  => '</div>',
            ) );
        else :
            the_excerpt();
        endif;
        ?>
    </div>

    <?php if ( is_singular() ) : ?>
        <footer class="entry-footer">
            <?php
            $categories = get_the_category_list( esc_html__( ', ', '{theme-slug}' ) );
            if ( $categories ) {
                printf(
                    '<span class="cat-links">%s %s</span>',
                    esc_html__( 'Posted in', '{theme-slug}' ),
                    $categories // Already escaped by get_the_category_list.
                );
            }

            $tags = get_the_tag_list( '', esc_html_x( ', ', 'tag list separator', '{theme-slug}' ) );
            if ( $tags ) {
                printf(
                    '<span class="tags-links">%s %s</span>',
                    esc_html__( 'Tagged', '{theme-slug}' ),
                    $tags // Already escaped by get_the_tag_list.
                );
            }
            ?>
        </footer>
    <?php endif; ?>
</article>
```

## Screen Reader Text CSS

```css
/* Screen reader text — required for accessibility. */
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

.screen-reader-text:focus {
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

## Read More with Post Title (Accessibility)

```php
// In template-tags.php or functions.php
function {prefix}_read_more_link() {
    return sprintf(
        '<a href="%1$s" class="more-link">%2$s<span class="screen-reader-text"> "%3$s"</span></a>',
        esc_url( get_permalink() ),
        esc_html__( 'Continue reading', '{theme-slug}' ),
        esc_html( get_the_title() )
    );
}
add_filter( 'the_content_more_link', '{prefix}_read_more_link' );

// For excerpts
function {prefix}_excerpt_more( $more ) {
    return '&hellip;';
}
add_filter( 'excerpt_more', '{prefix}_excerpt_more' );
```
