# WordPress Function: `body_class()` – Extended Obsidian Note

> **Note Type**: Reference / Template Tag / WordPress Core  
> **Tags**: #wordpress #php #theme-development #body_class #css #conditional-styling #accessibility  
> **Related Notes**: [[wp_head()]], [[post_class()]], [[get_body_class()]], [[header.php Template]], [[CSS Specificity]]  
> **Last Updated**: 2025-11-16  
> **Sources**: WordPress Developer Handbook, Core Source (`wp-includes/post-template.php`), Theme Handbook

---

## Why `body_class()` Was Created (150 words)

Before `body_class()`, theme developers manually added CSS classes to the `<body>` tag using hardcoded conditionals like `is_home()`, `is_single()`, or `is_category()`. This led to **duplicate code**, **maintenance nightmares**, and **inconsistent class names** across themes. Introduced in WordPress 2.8 (2009), `body_class()` solved this by automatically generating a **standardized, predictable set of CSS classes** based on the current page context. It centralizes logic in core, allowing themes to style pages conditionally with clean, semantic selectors like `.home`, `.single-post`, `.category-news`, or `.logged-in`. This dramatically improved **theme portability**, **plugin compatibility**, and **developer efficiency**. It also enabled advanced use cases: browser detection (`.chrome`), user state (`.admin-bar`), layout variations (`.has-sidebar`), and accessibility enhancements. Today, it’s a **required theme feature** and a cornerstone of modern WordPress theming.

---

## Overview Table

| **Left Column** | **Right Column** |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Function** | `body_class( string|array $class = '' )` |
| **Used In** | Must be placed in the opening `<body>` tag in `header.php` or equivalent template. |
| **Purpose** | Outputs a dynamic list of CSS classes on the `<body>` element based on current page type, user state, layout, and custom inputs. |
| **Why It Exists** | Eliminates manual conditional class logic in themes → ensures **consistency**, **maintainability**, and **extensibility** across all WordPress sites. |
| **What It Outputs** | Classes like: `home`, `blog`, `single`, `page`, `category-5`, `logged-in`, `admin-bar`, `no-customize-support`, `has-sidebar`, etc. |
| **Output Example** | `<body class="home blog logged-in admin-bar no-customize-support wp-embed-responsive">` |
| **Critical Placement** | Must be **inside the `<body>` tag** as an attribute: `<body <?php body_class(); ?>>`. |
| **Does NOT Affect** | Does **not** control content or behavior. Only adds **CSS hooks** for styling and JavaScript targeting. |
| **Usage** | `<?php body_class(); ?>` — echoes classes directly into the tag. |
| **Returns vs Echoes** | **Echoes by default**. Use `get_body_class( $class )` to **return** array of classes. |
| **Custom Classes** | Pass additional classes: `body_class( 'my-class another-class' )` or array. |
| **Filters** | `body_class` filter allows modifying or adding classes dynamically. |
| **Debug Note** | Missing `body_class()` breaks conditional styling, JavaScript targeting, admin bar alignment, and theme check compliance. |

---

## Full List of Default Classes (Core-Generated)

| Context | Example Classes |
|-------|-----------------|
| **Front Page** | `home`, `blog` (if blog posts index) |
| **Single Post** | `single`, `single-post`, `postid-123`, `single-format-standard` |
| **Page** | `page`, `page-id-2`, `page-template-default` |
| **Custom Post Type** | `single-book`, `archive-book` |
| **Category** | `category`, `category-news`, `category-5` |
| **Tag** | `tag`, `tag-wordpress`, `tag-12` |
| **Taxonomy** | `taxonomy-genre`, `genre-fiction` |
| **Author** | `author`, `author-john`, `author-1` |
| **Date** | `date`, `year-2025`, `month-11`, `day-16` |
| **Search** | `search`, `search-results`, `search-no-results` |
| **404** | `error404` |
| **Attachment** | `attachment`, `attachment-id-50`, `attachment-jpeg` |
| **Paged** | `paged`, `paged-2` |
| **User State** | `logged-in`, `admin-bar` (if logged in + top bar enabled) |
| **Layout** | `has-sidebar`, `no-sidebar`, `full-width` (theme-dependent) |
| **Browser** | `chrome`, `safari`, `firefox`, `mobile`, `tablet` (via `wp_is_mobile()`) |
| **Misc** | `rtl`, `custom-background`, `wp-embed-responsive`, `no-customize-support` |

> **View all live**: Right-click page → *Inspect* → check `<body>` classes.

---

## Echo vs Return: Key Difference

```php
<!-- ECHOES directly into attribute -->
<body <?php body_class(); ?>>

<!-- RETURNS array for logic -->
<?php $classes = get_body_class(); ?>
<?php if ( in_array( 'home', $classes ) ) : ?>
    <div class="hero-banner">Welcome!</div>
<?php endif; ?>
```

---

## Adding Custom Classes

### 1. Static String
```php
<body <?php body_class( 'my-theme light-mode' ); ?>>
```
→ Output: `... my-theme light-mode`

### 2. Array
```php
<body <?php body_class( [ 'my-theme', 'light-mode' ] ); ?>>
```

### 3. Conditional (Recommended)
```php
$extra = is_user_logged_in() ? 'premium-user' : 'guest';
body_class( $extra );
```

---

## Filter: Modify Classes Dynamically

```php
add_filter( 'body_class', 'my_custom_body_classes' );
function my_custom_body_classes( $classes ) {
    // Add custom class
    $classes[] = 'my-global-class';

    // Add based on user role
    if ( current_user_can( 'editor' ) ) {
        $classes[] = 'role-editor';
    }

    // Remove unwanted class
    if ( ( $key = array_search( 'no-customize-support', $classes ) ) !== false ) {
        unset( $classes[ $key ] );
    }

    return $classes;
}
```

---

## Real-World `header.php` Usage

```php
<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
    <meta charset="<?php bloginfo( 'charset' ); ?>">
    <?php wp_head(); ?>
</head>
<body <?php body_class(); ?>>
    <?php wp_body_open(); ?>
    <header class="site-header">
        ...
    </header>
```

> **Note**: `wp_body_open()` (since WP 5.2) fires right after `<body>` — use for early scripts.

---

## CSS Examples Using `body_class()`

```css
/* Front page hero */
.home .hero {
    background: url('home-bg.jpg') center/cover;
}

/* Hide sidebar on pages */
.page.no-sidebar .sidebar {
    display: none;
}

/* Dark mode for logged-in users */
.logged-in.dark-mode {
    filter: invert(1) hue-rotate(180deg);
}

/* Responsive admin bar offset */
.admin-bar .site-header {
    top: 32px;
}
@media (max-width: 782px) {
    .admin-bar .site-header { top: 46px; }
}
```

---

## Common Mistakes

| Mistake | Result | Fix |
|-------|--------|-----|
| `<?php body_class; ?>` (no `()`) | PHP error | Add parentheses |
| Placing outside `<body>` tag | Invalid HTML | Use `<body <?php body_class(); ?>>` |
| Hardcoding classes | Inflexible | Use `body_class()` + filter |
| Ignoring mobile classes | Poor mobile styling | Use `.mobile`, `.tablet` |

---

## Theme Checker Compliance

```php
// Must have in header.php
<body <?php body_class(); ?>>
```

> **Theme Check Plugin** will **fail** your theme without this.

---

## Performance Notes

- `body_class()` is **cached** and **lightweight**.
- Runs once per page load.
- Avoid expensive logic in `body_class` filter.

---

## Alternatives & Modern Use

| Need | Old Way | Modern Way |
|------|--------|-----------|
| Page-specific styling | `is_page('about')` in CSS | `.page-id-2` |
| User state | JS check | `.logged-in` |
| Layout control | PHP in template | `.has-sidebar` via filter |

---

## Debugging Tips

### 1. Print All Classes
```php
add_action('wp_footer', function() {
    if (current_user_can('administrator')) {
        echo '<pre style="background:#fff;padding:10px;">';
        print_r(get_body_class());
        echo '</pre>';
    }
});
```

### 2. Use Browser Dev Tools
- Inspect `<body>` → see live classes
- Toggle to test styling

---

## Links & Resources

- [Developer: `body_class()`](https://developer.wordpress.org/reference/functions/body_class/)
- [Source Code (GitHub)](https://github.com/WordPress/WordPress/blob/master/wp-includes/post-template.php#L600)
- [Theme Handbook: Template Tags](https://developer.wordpress.org/themes/basics/template-tags/)
- [[Obsidian Template: WP Template Tag]]

---

## Obsidian Canvas Connections

```
[body_class()] 
  → generates → [CSS hooks]
  → used in → [header.php]
  → filtered by → [body_class filter]
  → related → [post_class()], [wp_is_mobile()]
  → required for → [Theme Check Pass]
```

---

**Dataview Query** (in Obsidian):
```dataview
TABLE file.mtime AS "Updated"
FROM #body_class OR #css
SORT file.mtime DESC
```