# WordPress Function: `bloginfo()` – Extended Obsidian Note

> **Note Type**: Reference / PHP Function / WordPress Core  
> **Tags**: #wordpress #php #theme-development #bloginfo #site-info  
> **Related Notes**: [[get_bloginfo()]], [[wp_title()]], [[home_url()]], [[esc_html()]]  
> **Last Updated**: 2025-11-16  
> **Sources**: WordPress Codex (archived), Developer Handbook, Core Source (`wp-includes/general-template.php`)

---

## Overview Table

| **Left Column** | **Right Column** |
|-----------------|----------------------------------------------------------------------------------|
| **Function** | `bloginfo( string $show = '' )` |
| **Used In** | Themes (header, footer, templates), usually for showing core site info. |
| **Purpose** | Outputs specific site information such as site name, description, URL, charset, version, stylesheet URL, etc. |
| **Common Outputs** | `'name'` = Site Title • `'description'` = Tagline • `'charset'` = UTF-8 • `'url'` = Site URL • `'language'` = en-US |
| **Why It Exists** | Centralizes all site info in one place, so theme developers don’t hardcode values → ensures better **maintainability**, **SEO**, and **localization**. |
| **Output Example** | `<?php bloginfo( 'name' ); ?>` → *My WordPress Site* |
| **Determined From** | Most values come from **WP Admin → Settings → General**, others from internal WordPress settings. |
| **Does NOT Affect** | It does **not** modify site data. It only **outputs** stored settings (title, tagline, charset, version, etc.). |
| **Usage** | `<?php bloginfo( 'name' ); ?>` — echoes directly. |
| **Returns vs Echoes** | It **echoes by default**. Use `get_bloginfo( $show )` to **return** the value instead of printing it. |
| **Security Note** | Some outputs may need escaping, e.g., `esc_html( get_bloginfo( 'name' ) )`. |
| **Custom Filters** | Many keys have filters (e.g., `bloginfo_name`, `bloginfo_url`) allowing developers to override output. |

---

## Full List of Valid `$show` Parameters

| Parameter | Returns | Source | Example Output |
|---------|--------|--------|----------------|
| `name` | Site title | Settings → General | `My Awesome Blog` |
| `description` | Tagline | Settings → General | `Just another WordPress site` |
| `url` / `home` | Home URL | `home_url()` filterable | `https://example.com` |
| `wpurl` | WordPress address (install URL) | `site_url()` | `https://example.com/wp` |
| `stylesheet_url` | Main stylesheet URL | Child/parent theme logic | `https://example.com/wp-content/themes/twentytwentyfive/style.css` |
| `stylesheet_directory` | Stylesheet directory URI | Same as above | `https://example.com/wp-content/themes/twentytwentyfive` |
| `template_url` / `template_directory` | Parent template directory URI | Falls back if no child | Same as above if no child theme |
| `admin_email` | Admin email | Settings → General | `admin@example.com` |
| `charset` | Content charset | `get_option('blog_charset')` | `UTF-8` |
| `html_type` | HTML content type | `get_option('html_type')` | `text/html` |
| `version` | WordPress version | `$wp_version` global | `6.7.1` |
| `language` | Site language | `get_locale()` | `en_US` |
| `text_direction` | `ltr` or `rtl` | `is_rtl()` check | `ltr` |
| `pingback_url` | XML-RPC pingback URL | `site_url('xmlrpc.php')` | `https://example.com/xmlrpc.php` |
| `rss_url` | Main RSS feed | `get_feed_link('rss')` | `https://example.com/feed/` |
| `rss2_url` | RSS2 feed | `get_feed_link('rss2')` | `https://example.com/feed/` |
| `atom_url` | Atom feed | `get_feed_link('atom')` | `https://example.com/feed/atom/` |
| `comments_rss2_url` | Comments RSS2 | `get_feed_link('comments_rss2')` | `https://example.com/comments/feed/` |
| `rdf_url` | RDF feed | Legacy | `https://example.com/feed/rdf/` |
| `name` (with filter) | Filterable via `bloginfo_name` | Hookable | Custom via plugin |

> **Tip**: Use `get_bloginfo()` in conditionals or concatenations:  

```php
<title><?php echo esc_html( get_bloginfo( 'name' ) ); ?> | <?php echo wp_get_document_title(); ?></title>
```

---

## Echo vs Return: Critical Difference

```php
// ECHOES directly to output
bloginfo( 'name' ); 
// → My Awesome Blog

// RETURNS string for use in code
$site_name = get_bloginfo( 'name' );
// → Assigns "My Awesome Blog" to variable
```

> **Best Practice**:  
> Always use `get_bloginfo()` when you need to **manipulate**, **escape**, or **store** the value.

---

## Security & Escaping Guide

| Output Type | Risk | Recommended Escaping |
|-----------|------|------------------------|
| Site name/description | User input → XSS possible | `esc_html( get_bloginfo('name') )` |
| URLs | Injection if misused | `esc_url( get_bloginfo('url') )` |
| Charset / version | Low risk | Usually safe unescaped |
| Email | Spam / phishing display | `antispambot( get_bloginfo('admin_email') )` |

```php
<!-- Safe title tag -->
<title><?php echo esc_html( get_bloginfo( 'name' ) ); ?> - <?php echo esc_html( get_bloginfo( 'description' ) ); ?></title>

<!-- Safe mailto link -->
<a href="mailto:<?php echo antispambot( get_bloginfo( 'admin_email' ) ); ?>">
    Contact Us
</a>
```

---

## Filters & Hooks (Advanced Customization)

Each output can be filtered:

```php
// Change site name dynamically
add_filter( 'bloginfo_name', function( $output ) {
    return $output . ' [Beta]';
} );

// Override URL for staging
add_filter( 'bloginfo_url', function( $url ) {
    if ( wp_get_environment_type() === 'staging' ) {
        return 'https://staging.example.com';
    }
    return $url;
} );
```

> See all filters in source: [`apply_filters( "bloginfo_{$show}", ... )`](https://github.com/WordPress/WordPress/blob/master/wp-includes/general-template.php)

---

## Real-World Theme Examples

### `header.php`
```php
<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
    <meta charset="<?php bloginfo( 'charset' ); ?>">
    <meta name="description" content="<?php echo esc_attr( get_bloginfo( 'description' ) ); ?>">
    <title><?php echo wp_title( '|', false, 'right' ) . get_bloginfo( 'name' ); ?></title>
    <link rel="stylesheet" href="<?php bloginfo( 'stylesheet_url' ); ?>">
    <?php wp_head(); ?>
</head>
```

### `footer.php`
```php
<footer>
    <p>&copy; <?php echo date('Y'); ?> <?php bloginfo('name'); ?>. All rights reserved.</p>
    <p><a href="<?php bloginfo('rss2_url'); ?>">RSS Feed</a></p>
</footer>
<?php wp_footer(); ?>
```

---

## Common Mistakes

| Mistake | Result | Fix |
|-------|--------|-----|
| Using `bloginfo('url')` in `href` without escaping | Broken links or XSS | Use `echo esc_url( home_url() )` instead |
| Hardcoding charset | Breaks on multisite or custom setups | Always use `bloginfo('charset')` |
| Using `bloginfo()` in JavaScript | Output before `<script>` → syntax error | Use `wp_localize_script()` |
| Assuming `get_bloginfo('url')` = current page | It’s *home* URL, not current | Use `get_permalink()` or `home_url( add_query_arg(...) )` |

---

## Performance Notes

- `bloginfo()` / `get_bloginfo()` are **lightweight**.
- Values are cached in options table.
- Avoid calling inside loops unnecessarily.

---

## Alternatives & Modern Replacements

| Need | Old Way | Modern Way |
|------|---------|------------|
| Site URL | `bloginfo('url')` | `home_url()` |
| Stylesheet URL | `bloginfo('stylesheet_url')` | `get_stylesheet_uri()` |
| Template URL | `bloginfo('template_url')` | `get_template_directory_uri()` |
| Site Name | `bloginfo('name')` | `get_bloginfo('name')` (still fine) |

> **Note**: `bloginfo()` remains valid and widely used — no deprecation planned.

---

## Debugging Tips

```php
// Print all available bloginfo keys
global $wp_bloginfo_keys;
print_r( array_keys( $wp_bloginfo_keys ) );
```

Or use WP-CLI:
```bash
wp eval 'echo get_bloginfo("name");'
```

---

## Links & Resources

- [WordPress Developer: `bloginfo()`](https://developer.wordpress.org/reference/functions/bloginfo/)
- [Source Code (GitHub)](https://github.com/WordPress/WordPress/blob/master/wp-includes/general-template.php#L200)
- [[Obsidian Template: WP Function Reference]]

---

**Obsidian Canvas Tip**: Drag this note into a canvas and connect:
```
[bloginfo()] → [get_bloginfo()] → [Filters] → [Security]
```

---
```