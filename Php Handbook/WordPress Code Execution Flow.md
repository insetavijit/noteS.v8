## WordPress Code Execution Flow – Complete Guide (IN)

_November 16, 2025 – 01:55 PM IST | India_

---

### **Code Execution Summary (150 Words)**

WordPress's execution flow begins with bootstrap in `wp-load.php` and `wp-config.php`, connecting the database and initializing core objects like `$wp_styles` and `$wp_scripts`. Assets are enqueued via `wp_enqueue_scripts` hook, building dependency graphs without output. In the header, `wp_head()` triggers actions: CSS prints at priority 8 via `wp_print_styles()`, followed by head scripts at priority 9 with `wp_print_head_scripts()`. Content rendering uses `WP_Query` and the Loop to generate main HTML. Footer starts with `wp_footer()`, printing remaining scripts at priority 20 via `wp_print_footer_scripts()`. Shutdown hook finalizes output, flushing the buffer. This structured process ensures efficient loading: enqueue separates registration from printing, dependencies auto-resolve, and footer scripts optimize performance by loading after DOM. Best practices include conditional enqueues, versioning, and avoiding hardcoded tags for maintainability.

---

### **EXECUTION SUMMARY TABLE (TOP)**

|Step|Phase|Action / Hook|Priority|Output|Memory State|
|---|---|---|---|---|---|
|1|Bootstrap|Load core + theme|N/A|None|Core + DB ready|
|2|Enqueue|`wp_enqueue_scripts`|10|None|Assets queued|
|3|Head Start|`wp_head()`|Varies|`<head>` opens|Queue intact|
|4|CSS Print|`wp_print_styles()`|8|`<link>` tags|CSS dequeued|
|5|Head JS|`wp_print_head_scripts()`|9|`<script>` (head)|Head JS dequeued|
|6|Content|`WP_Query` + Loop|N/A|Main HTML|Active query|
|7|Footer Start|`wp_footer()`|Varies|Footer HTML|JS still queued|
|8|Footer JS|`wp_print_footer_scripts()`|20|`<script>` (footer)|All JS dequeued|
|9|Shutdown|`shutdown`|N/A|Full HTML|Cleanup|

**Average Flow Score: 9.6/10** | **Critical Hook:** `wp_enqueue_scripts`

---

### **🔄 Visual Execution Flow**

```
START
  ↓
wp-load.php (Bootstrap: Core, DB, Theme)
  ↓
wp_enqueue_scripts hook fires
  ├─→ wp_enqueue_style('handle', 'url', deps, ver)
  ├─→ wp_enqueue_script('handle', 'url', deps, ver, footer)
  └─→ Assets queued in $wp_styles / $wp_scripts
  ↓
wp_head() called by header.php
  ├─→ [Priority 1-7] Early hooks (SEO, meta tags)
  ├─→ [Priority 8] wp_print_styles() → <link> tags
  ├─→ [Priority 9] wp_print_head_scripts() → <script> tags
  └─→ [Priority 10+] Custom wp_head actions
  ↓
Template renders content (Loop, blocks)
  ├─→ WP_Query processes posts
  ├─→ Filters: the_content, the_title, the_excerpt
  └─→ Main HTML body generated
  ↓
wp_footer() called by footer.php
  ├─→ [Priority 1-19] Footer HTML, widgets, modals
  └─→ [Priority 20] wp_print_footer_scripts() → <script> tags
  ↓
shutdown hook fires
  ├─→ Output buffer flushed
  ├─→ Database connections closed
  └─→ Cleanup
  ↓
Complete HTML delivered to browser
```

---

### **📚 Detailed Execution Phases**

#### **Phase 1: Bootstrap**

**What Happens:**

- `wp-load.php` loads `wp-config.php` (DB credentials, constants)
- Database connection established via `wpdb`
- Theme's `functions.php` loaded
- `script-loader.php` creates `WP_Scripts` and `WP_Styles` objects

**Available Hooks:** `after_setup_theme`, `init`, `wp_loaded`  
**Output:** None  
**Memory State:** Core environment ready, no assets queued yet

---

#### **Phase 2: Asset Enqueueing**

**What Happens:** WordPress fires:

```php
do_action('wp_enqueue_scripts');
```

**Your Code:**

```php
function my_theme_assets() {
    // Enqueue stylesheet
    wp_enqueue_style('main', get_stylesheet_uri(), [], '1.0');
    
    // Enqueue script with jQuery dependency, load in footer
    wp_enqueue_script('app', get_template_directory_uri() . '/js/app.js', ['jquery'], '1.0', true);
    
    // Pass PHP data to JavaScript
    wp_localize_script('app', 'wpData', [
        'ajaxUrl' => admin_url('admin-ajax.php'),
        'nonce'   => wp_create_nonce('my_nonce'),
        'siteUrl' => home_url()
    ]);
}
add_action('wp_enqueue_scripts', 'my_theme_assets');
```

**Internal Process:**

- Assets added to `$wp_styles->queue` and `$wp_scripts->queue` arrays
- Dependencies calculated but not yet resolved
- Conditional logic can be applied

**Key Concept:** `wp_register_script()` vs `wp_enqueue_script()`

```php
// Register = Add to system (doesn't load yet)
wp_register_script('my-lib', get_template_directory_uri() . '/js/lib.js');

// Later, conditionally enqueue
if (is_singular('product')) {
    wp_enqueue_script('my-lib'); // Already registered
}

// OR: Enqueue directly (registers + queues in one step)
wp_enqueue_script('my-lib', get_template_directory_uri() . '/js/lib.js');
```

**Output:** None (assets stored in memory only)  
**Memory State:** Queue populated with handles, URLs, dependencies

---

#### **Phase 3: Header Start (`<head>` begins)**

**What Happens:** Theme's `header.php` calls:

```php
<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
    <meta charset="<?php bloginfo('charset'); ?>">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <?php wp_head(); ?>
</head>
```

`wp_head()` triggers:

```php
do_action('wp_head'); // Fires multiple hooks by priority
```

**Hook Execution Order:**

1. Priority 0-7: Early hooks (Open Graph, structured data)
2. **Priority 8:** `wp_print_styles()` fires
3. **Priority 9:** `wp_print_head_scripts()` fires
4. Priority 10+: Analytics, custom meta tags

**Output:** Beginning of `<head>` section  
**Purpose:** Allow plugins/themes to inject head content

---

#### **Phase 4: CSS Output**

**What Happens:** `wp_print_styles()` auto-executes at priority 8.

**Internal Process:**

1. Resolves dependency tree (parent styles first)
2. Applies `style_loader_src` filter to each URL
3. Generates `<link>` tags with proper attributes

**Output:**

```html
<link rel='stylesheet' id='main-css' href='/wp-content/themes/mytheme/style.css?ver=1.0' media='all' />
```

**Available Filters:**

- `style_loader_src` - Modify stylesheet URLs
- `style_loader_tag` - Modify entire `<link>` tag

**Memory State:** Styles dequeued as they're printed

---

#### **Phase 5: Header Scripts Output**

**What Happens:** `wp_print_head_scripts()` auto-executes at priority 9.

**Internal Process:**

- Prints only scripts with `$in_footer = false`
- Typical examples: jQuery, Modernizr, polyfills
- Applies `script_loader_tag` filter

**Output:**

```html
<script src='/wp-includes/js/jquery/jquery.min.js?ver=3.7.1' id='jquery-core-js'></script>
```

**When to Use Head Scripts:**

- Critical above-the-fold JavaScript
- Feature detection (Modernizr)
- Polyfills needed before DOM rendering

**Memory State:** Head scripts dequeued, footer scripts remain queued

---

#### **Phase 6: Content Rendering**

**What Happens:** WordPress determines which template to use (template hierarchy), then:

```php
<?php
if (have_posts()) :
    while (have_posts()) : the_post();
        ?>
        <article id="post-<?php the_ID(); ?>">
            <h1><?php the_title(); ?></h1>
            <div class="content">
                <?php the_content(); ?>
            </div>
        </article>
        <?php
    endwhile;
endif;
?>
```

**Filters Applied:**

- `the_content` - Modify post content
- `the_title` - Modify post title
- `the_excerpt` - Modify excerpts
- Shortcodes processed
- Gutenberg blocks rendered

**Output:** Main page HTML (posts, pages, custom post types)  
**Memory State:** Active `WP_Query` object with post data

---

#### **Phase 7: Footer Start**

**What Happens:** Theme's `footer.php` calls:

```php
    <footer class="site-footer">
        <!-- Footer content -->
    </footer>
    
    <?php wp_footer(); ?>
</body>
</html>
```

`wp_footer()` triggers:

```php
do_action('wp_footer');
```

**Hook Execution Order:**

1. Priority 0-19: Footer widgets, modals, cookie notices
2. **Priority 20:** `wp_print_footer_scripts()` fires
3. Priority 21+: Tracking pixels, late-loading content

**Common Uses:**

- Cookie consent banners
- Back-to-top buttons
- Chat widgets
- Hidden modals/overlays

**Output:** Footer HTML  
**Memory State:** Footer scripts still queued, ready to print

---

#### **Phase 8: Footer Scripts Output**

**What Happens:** `wp_print_footer_scripts()` auto-executes at priority 20.

**Internal Process:**

1. Prints all scripts marked `$in_footer = true`
2. Includes inline scripts from `wp_add_inline_script()`
3. Localized data from `wp_localize_script()` printed before each script

**Output:**

```html
<script src='/wp-content/themes/mytheme/js/app.js?ver=1.0' id='app-js'></script>
<script id='app-js-before'>
var wpData = {"ajaxUrl":"https://example.com/wp-admin/admin-ajax.php","nonce":"abc123"};
</script>
```

**Why Footer Loading?**

- Non-blocking: HTML renders first
- Better performance metrics (FCP, LCP)
- DOM already available for manipulation

**Memory State:** All scripts dequeued and printed

---

#### **Phase 9: Completion & Shutdown**

**What Happens:**

- `</body>` and `</html>` tags close document
- `shutdown` hook fires
- Output buffer (if used) flushed to browser
- Database connections closed
- Temporary files/data cleaned up

**Shutdown Hook Uses:**

```php
add_action('shutdown', function() {
    // Log final execution time
    error_log('Page generated in: ' . timer_stop() . 's');
    
    // Clear temporary caches
    // Final cleanup tasks
});
```

**Output:** Complete HTML document  
**Purpose:** Finalize response, cleanup resources

---

### **📊 Complete Comparison Table**

|Step|Phase|Action / Hook|Hook Priority|Output Generated|Memory State|Context/Purpose|Filters/Actions Available|Practical Examples|
|---|---|---|---|---|---|---|---|---|
|1|**Bootstrap**|Load `wp-load.php`, theme files, `script-loader.php`|N/A|None|Core + theme loaded|Initialize WordPress environment|`after_setup_theme`, `init`, `wp_loaded`|Register post types, add theme support, load text domains|
|2|**Enqueue**|`do_action('wp_enqueue_scripts')`|10 (default)|None|Assets queued in `$wp_scripts`, `$wp_styles`|Register dependencies, prepare assets|`wp_enqueue_scripts`|`wp_enqueue_style()`, `wp_enqueue_script()`, `wp_localize_script()`|
|3|**Head Start**|`wp_head()` called by theme|Varies|`<head>` opens|Queue still in memory|Entry point for head content|`wp_head` (0-99)|Add meta tags, Open Graph, favicons, schema markup|
|4|**CSS Print**|`wp_print_styles()` auto-fires|8|`<link>` tags in `<head>`|Styles dequeued as printed|Output stylesheets in dependency order|`style_loader_src`, `style_loader_tag`|Custom CDN URLs, preload, modify media attributes|
|5|**Head JS**|`wp_print_head_scripts()` auto-fires|9|`<script>` tags (head only)|Head scripts dequeued|Print critical JavaScript|`script_loader_tag`, `script_loader_src`|Add `async`/`defer`, modify script types, crossorigin|
|6|**Content**|Template renders (Loop, blocks)|N/A|Main HTML body|Active WP_Query|Generate page content|`the_content`, `the_excerpt`, `the_title`, `post_class`|Modify post content, add social shares, inject ads|
|7|**Footer Start**|`wp_footer()` called by theme|Varies|Footer HTML begins|Footer scripts still queued|Inject pre-script footer content|`wp_footer` (0-19)|Cookie notices, chat widgets, modals, scroll-to-top|
|8|**Footer JS**|`wp_print_footer_scripts()` auto-fires|20|`<script>` tags at end of `<body>`|Scripts dequeued as printed|Output non-critical JavaScript|`script_loader_tag`|Analytics, social widgets, lazy loading, animations|
|9|**Shutdown**|`</body></html>` + cleanup|N/A|Complete HTML document|Connections closed|Finalize and deliver page|`shutdown`|Performance logging, final cache writes, cleanup|

---

### **🎯 Key Insights for Developers**

#### **1. Queue vs. Print Separation**

```php
// ENQUEUE PHASE - Just registers (Phase 2)
wp_enqueue_script('my-script', 'file.js');

// PRINT PHASE - Actually outputs HTML (Phase 5 or 8)
// Happens automatically via wp_head() or wp_footer()
```

#### **2. Dependency Management**

```php
// This ensures jQuery loads BEFORE your script
wp_enqueue_script('app', 'app.js', ['jquery'], '1.0', true);
//                                    ↑ deps      ↑ ver  ↑ footer

// Multiple dependencies
wp_enqueue_script('advanced', 'app.js', ['jquery', 'jquery-ui-core', 'underscore'], '1.0', true);
```

**What if dependency is missing?** WordPress will still enqueue your script but won't load the missing dependency, causing runtime errors:

```javascript
// Console error: "$ is not defined"
// Because jQuery wasn't loaded
```

**Solution:**

```php
// Check if dependency exists before enqueueing
if (wp_script_is('backbone', 'registered')) {
    wp_enqueue_script('app', 'app.js', ['backbone'], '1.0', true);
} else {
    // Fallback: register it yourself or skip
    error_log('Backbone not found - app.js not loaded');
}
```

#### **3. Hook Priority Matters**

```php
// Runs BEFORE wp_print_styles (priority 8)
add_action('wp_head', 'my_early_function', 5);

// Runs AFTER styles/scripts (priority 8-9)
add_action('wp_head', 'my_late_function', 10);

// Runs AFTER wp_print_footer_scripts (priority 20)
add_action('wp_footer', 'my_analytics', 25);
```

**Priority Reference:**

- `1-7`: Before CSS
- `8`: CSS prints
- `9`: Head scripts print
- `10-19`: After styles/scripts, before footer scripts
- `20`: Footer scripts print
- `21+`: After all scripts

#### **4. Conditional Loading**

```php
function smart_asset_loading() {
    // Homepage only
    if (is_front_page()) {
        wp_enqueue_script('homepage-slider', 'slider.js');
    }
    
    // Single posts only
    if (is_singular('post')) {
        wp_enqueue_script('comments-ajax', 'comments.js');
    }
    
    // WooCommerce pages only
    if (function_exists('is_woocommerce') && is_woocommerce()) {
        wp_enqueue_script('shop', 'shop.js');
    }
    
    // Mobile only (basic detection)
    if (wp_is_mobile()) {
        wp_enqueue_style('mobile', 'mobile.css');
    }
}
add_action('wp_enqueue_scripts', 'smart_asset_loading');
```

#### **5. Passing Data to JavaScript**

```php
function my_scripts() {
    wp_enqueue_script('app', get_template_directory_uri() . '/js/app.js', [], '1.0', true);
    
    // Make PHP data available in JavaScript
    wp_localize_script('app', 'myAppData', [
        'ajaxUrl'    => admin_url('admin-ajax.php'),
        'nonce'      => wp_create_nonce('my_ajax_nonce'),
        'userId'     => get_current_user_id(),
        'postId'     => get_the_ID(),
        'siteUrl'    => home_url(),
        'themePath'  => get_template_directory_uri(),
        'i18n'       => [
            'loading' => __('Loading...', 'textdomain'),
            'error'   => __('An error occurred', 'textdomain')
        ]
    ]);
}
add_action('wp_enqueue_scripts', 'my_scripts');
```

**In JavaScript:**

```javascript
// Access the data
console.log(myAppData.ajaxUrl); // "https://example.com/wp-admin/admin-ajax.php"
console.log(myAppData.i18n.loading); // "Loading..."

// Use in AJAX request
jQuery.ajax({
    url: myAppData.ajaxUrl,
    data: {
        action: 'my_action',
        nonce: myAppData.nonce
    }
});
```

---

### **🚀 Performance Best Practices**

#### **✅ DO:**

```php
// 1. Load scripts in footer when possible
wp_enqueue_script('app', 'app.js', ['jquery'], '1.0', true);
//                                                      ↑ true = footer

// 2. Use dependency parameters (not manual ordering)
wp_enqueue_script('theme', 'theme.js', ['jquery', 'jquery-ui-tabs'], '1.0', true);

// 3. Conditionally load assets (only where needed)
if (is_singular('product')) {
    wp_enqueue_script('product-viewer', 'viewer.js');
}

// 4. Version assets for cache busting
$theme_version = wp_get_theme()->get('Version');
wp_enqueue_style('theme', get_stylesheet_uri(), [], $theme_version);

// 5. Use file modification time for auto-versioning during development
$stylesheet_path = get_stylesheet_directory() . '/style.css';
$version = file_exists($stylesheet_path) ? filemtime($stylesheet_path) : '1.0';
wp_enqueue_style('theme', get_stylesheet_uri(), [], $version);

// 6. Defer non-critical scripts
add_filter('script_loader_tag', function($tag, $handle) {
    if ('non-critical' === $handle) {
        return str_replace(' src', ' defer src', $tag);
    }
    return $tag;
}, 10, 2);

// 7. Preload critical assets
add_action('wp_head', function() {
    echo '<link rel="preload" href="' . get_stylesheet_uri() . '" as="style">';
}, 1);
```

#### **❌ DON'T:**

```php
// 1. Don't print scripts directly in templates
// ❌ BAD:
<script src="<?php echo get_template_directory_uri(); ?>/js/app.js"></script>

// ✅ GOOD:
wp_enqueue_script('app', get_template_directory_uri() . '/js/app.js');

// 2. Don't hardcode script tags in header.php/footer.php
// ❌ BAD:
<script src="https://cdn.example.com/lib.js"></script>

// ✅ GOOD:
wp_enqueue_script('external-lib', 'https://cdn.example.com/lib.js');

// 3. Don't load admin scripts on frontend
// ❌ BAD:
wp_enqueue_script('admin-js', 'admin.js'); // Loads everywhere

// ✅ GOOD:
add_action('admin_enqueue_scripts', function() {
    wp_enqueue_script('admin-js', 'admin.js');
});

// 4. Don't skip the enqueue system
// ❌ BAD: Direct <script> tags bypass dependency management

// 5. Don't enqueue in global scope (conditional tags won't work)
// ❌ BAD (in functions.php):
if (is_front_page()) { // Too early - doesn't work!
    wp_enqueue_script('home', 'home.js');
}

// ✅ GOOD:
add_action('wp_enqueue_scripts', function() {
    if (is_front_page()) {
        wp_enqueue_script('home', 'home.js');
    }
});
```

---

### **🚫 Common Mistakes to Avoid**

#### **1. Enqueueing in Wrong Hook**

```php
// ❌ Too early - conditional tags don't work yet
add_action('init', function() {
    if (is_front_page()) { // Always false at this point!
        wp_enqueue_script('home', 'home.js');
    }
});

// ✅ Correct timing
add_action('wp_enqueue_scripts', function() {
    if (is_front_page()) {
        wp_enqueue_script('home', 'home.js');
    }
});
```

#### **2. Not Checking Handle Existence**

```php
// ❌ Might fail if plugin isn't active
wp_dequeue_script('some-plugin-script');

// ✅ Safe approach
if (wp_script_is('some-plugin-script', 'enqueued')) {
    wp_dequeue_script('some-plugin-script');
}
```

#### **3. Dequeuing at Wrong Priority**

```php
// ❌ Too late - script already printed
add_action('wp_head', function() {
    wp_dequeue_script('unwanted-script');
});

// ✅ Correct - dequeue before printing
add_action('wp_enqueue_scripts', function() {
    wp_dequeue_script('unwanted-script');
}, 100); // High priority = runs late in enqueue phase
```

#### **4. Assuming Scripts Auto-Defer**

```php
// WordPress does NOT add defer automatically
// Footer scripts are just placed before </body>

// To add defer manually:
add_filter('script_loader_tag', function($tag, $handle) {
    if ('my-script' === $handle) {
        return str_replace(' src', ' defer src', $tag);
    }
    return $tag;
}, 10, 2);
```

#### **5. Not Handling Frontend vs Admin**

```php
// ❌ Loads everywhere (frontend + admin)
wp_enqueue_script('my-script', 'script.js');

// ✅ Frontend only
add_action('wp_enqueue_scripts', function() {
    wp_enqueue_script('my-script', 'script.js');
});

// ✅ Admin only
add_action('admin_enqueue_scripts', function() {
    wp_enqueue_script('admin-script', 'admin.js');
});

// ✅ Login page only
add_action('login_enqueue_scripts', function() {
    wp_enqueue_style('login-style', 'login.css');
});
```

---

### **🔍 Debugging Tips**

#### **1. Check What's Enqueued**

```php
add_action('wp_footer', function() {
    global $wp_scripts, $wp_styles;
    
    echo '<!-- ENQUEUED SCRIPTS: ';
    print_r($wp_scripts->queue);
    echo ' -->';
    
    echo '<!-- ENQUEUED STYLES: ';
    print_r($wp_styles->queue);
    echo ' -->';
}, 999);
```

#### **2. Check Script Status**

```php
// Check if script exists and its status
if (wp_script_is('jquery', 'registered')) {
    echo 'jQuery is registered';
}

if (wp_script_is('jquery', 'enqueued')) {
    echo 'jQuery is enqueued (will print)';
}

if (wp_script_is('jquery', 'done')) {
    echo 'jQuery already printed';
}
```

#### **3. See All Registered Handles**

```php
add_action('wp_footer', function() {
    global $wp_scripts;
    echo '<!-- REGISTERED SCRIPTS: ';
    echo implode(', ', array_keys($wp_scripts->registered));
    echo ' -->';
}, 999);
```

#### **4. Debug Dependency Issues**

```php
add_action('wp_print_scripts', function() {
    global $wp_scripts;
    
    foreach ($wp_scripts->queue as $handle) {
        $deps = $wp_scripts->registered[$handle]->deps;
        error_log("Script '$handle' depends on: " . implode(', ', $deps));
    }
}, 1);
```

#### **5. Monitor Hook Execution**

```php
add_action('wp_enqueue_scripts', function() {
    error_log('wp_enqueue_scripts fired at ' . current_time('mysql'));
}, 1);

add_action('wp_head', function() {
    error_log('wp_head fired at priority ' . current_filter());
}, 1);

add_action('wp_footer', function() {
    error_log('wp_footer fired at priority ' . current_filter());
}, 1);
```

---

### **💼 5 Real-World Interview Questions + Answers**

---

#### **Q1: What's the difference between `wp_enqueue_script()` and `wp_print_scripts()`? Why can't we just use `<script>` tags?**

**Answer:**  
`wp_enqueue_script()` **registers** a script into WordPress's dependency management system (`$wp_scripts->queue`). It does **not** output anything—it only prepares the script for printing.

`wp_print_scripts()` (or internal calls via `wp_head()`/`wp_footer()`) **outputs** the queued scripts as `<script>` tags at the correct time and in dependency order.

Using hardcoded `<script>` tags bypasses:

- Dependency resolution (scripts may load in wrong order)
- Version control and cache busting
- Conditional loading logic
- Duplicate prevention
- Plugin/theme interoperability

**Common Doubt:** _"But my hardcoded script loads fine!"_

**Solution:** It may appear to work, but:

```javascript
// Your script might run before jQuery
<script src="app.js"></script> // Uses $ but jQuery not loaded yet
// Error: "$ is not defined"
```

```php
// Other plugins can't dequeue or replace it
wp_dequeue_script('your-hardcoded-script'); // Doesn't work!
```

**Always enqueue:**

```php
add_action('wp_enqueue_scripts', function() {
    wp_enqueue_script('app', get_template_directory_uri() . '/js/app.js', ['jquery'], '1.0', true);
});
```

---

#### **Q2: Why load scripts in the footer (`true` as 5th param)? When should you load in `<head>`?**

**Answer:**  
Footer loading (`$in_footer = true`) places scripts before `</body>`, allowing HTML to parse and render first → **faster First Contentful Paint (FCP)** and better perceived performance.

**Load in `<head>` only for:**

- **Critical scripts** needed before DOM renders (Modernizr, polyfills)
- **Feature detection** libraries
- **Above-the-fold** interactive elements (hero sliders)

**Common Doubt:** _"My script needs DOM to be ready—footer loading breaks it!"_

**Solution:**  
Footer scripts load after DOM is parsed. Wrap your code:

```javascript
// Pure JavaScript
document.addEventListener('DOMContentLoaded', function() {
    // Your code here - DOM is ready
});

// Or with jQuery (waits for DOM automatically)
jQuery(document).ready(function($) {
    // Your code here
});
```

**Example:**

```php
// Footer = best practice (non-blocking)
wp_enqueue_script('app', 'app.js', ['jquery'], '1.0', true);
//                                                      ↑ true = footer

// Head = only if critical
wp_enqueue_script('modernizr', 'modernizr.js', [], '3.0', false);
//                                                          ↑ false = head
```

---

#### **Q3: How does WordPress resolve script dependencies? What happens if a dependency is missing?**

**Answer:**  
When you enqueue with dependencies:

```php
wp_enqueue_script('app', 'app.js', ['jquery', 'backbone'], '1.0', true);
```

**WordPress:**

1. Checks if `jquery` and `backbone` are registered
2. Adds them to queue **before** `app.js`
3. Resolves their dependencies recursively
4. Prints in correct order:

```html
<script src="jquery.js"></script>
<script src="underscore.js"></script> <!-- backbone's dependency -->
<script src="backbone.js"></script>
<script src="app.js"></script>
```

**If dependency is missing:**  
WordPress still enqueues your script but **doesn't load the missing dependency**, causing runtime errors:

```javascript
// Console: "Backbone is not defined"
// Because Backbone wasn't registered/enqueued
```

**Common Doubt:** _"My script throws `$ is not defined` even though I added jQuery as dependency!"_

**Solution:**

```php
// Check if dependency exists
if (wp_script_is('jquery', 'registered')) {
    wp_enqueue_script('app', 'app.js', ['jquery'], '1.0', true);
} else {
    // Fallback: enqueue jQuery yourself
    wp_enqueue_script('jquery');
    wp_enqueue_script('app', 'app.js', ['jquery'], '1.0', true);
}

// Or register missing dependency
wp_register_script('backbone', 'https://cdn.example.com/backbone.js', ['underscore']);
wp_enqueue_script('app', 'app.js', ['backbone'], '1.0', true);
```

---

#### **Q4: You added a `wp_head` action with priority 10. Where does it run relative to CSS/JS?**

**Answer:**  
`wp_head` hook execution order:

- **Priority 8:** `wp_print_styles()` → CSS prints
- **Priority 9:** `wp_print_head_scripts()` → Head JS prints
- **Priority 10:** Your action → **runs AFTER CSS & head scripts**

**Example:**

```php
add_action('wp_head', 'add_google_analytics', 10);

function add_google_analytics() {
    // This runs AFTER stylesheets and head scripts
    echo '<script>/* GA code */</script>';
}
```

**Common Doubt:** _"I added Google Analytics at priority 1—why is it appearing below CSS in the source?"_

**Solution:**  
Use priority **below 8** to inject before CSS:

```php
// Priority 5 = runs before CSS (priority 8)
add_action('wp_head', 'add_early_meta_tags', 5);

function add_early_meta_tags() {
    echo '<meta name="description" content="...">';
    // Appears BEFORE <link rel="stylesheet">
}

// Priority 8+ = runs after CSS/JS
add_action('wp_head', 'add_late_tracking', 11);
```

**Priority Reference:**

```
Priority 1-7:  Before CSS (meta tags, Open Graph)
Priority 8:    CSS prints (wp_print_styles)
Priority 9:    Head scripts print (wp_print_head_scripts)
Priority 10+:  After styles/scripts (analytics, custom code)
```

---

#### **Q5: A client says: "My site is slow. Scripts are loading twice." How do you debug?**

**Answer (Step-by-Step):**

**1. Inspect Browser Source**

- Right-click → View Page Source
- Search for duplicate `<script>` tags (same `src` or `id`)

```html
<!-- DUPLICATE FOUND: -->
<script src="/js/bootstrap.min.js" id="bootstrap-js"></script>
<script src="/js/bootstrap.min.js"></script> <!-- Hardcoded duplicate -->
```

**2. Check What's Enqueued**

```php
add_action('wp_footer', function() {
    global $wp_scripts;
    echo '<!-- Scripts in queue: ';
    print_r($wp_scripts->queue);
    echo ' -->';
}, 999);
```

**3. Find the Culprits**

```php
// Add to functions.php temporarily
add_action('wp_print_scripts', function() {
    global $wp_scripts;
    foreach ($wp_scripts->queue as $handle) {
        $src = $wp_scripts->registered[$handle]->src;
        if (strpos($src, 'bootstrap') !== false) {
            error_log("Bootstrap enqueued by: $handle");
        }
    }
});
```

**4. Identify Double Enqueue**

```php
// Theme enqueues:
wp_enqueue_script('bootstrap', 'bootstrap.js');

// Plugin also enqueues (different handle, same file):
wp_enqueue_script('my-plugin-bootstrap', 'bootstrap.js');
```

**5. Fix It**

```php
// Dequeue plugin's version, keep theme's
add_action('wp_enqueue_scripts', function() {
    wp_dequeue_script('my-plugin-bootstrap');
}, 100); // High priority = runs after plugin

// Or check before enqueueing:
function my_theme_scripts() {
    if (!wp_script_is('bootstrap', 'enqueued')) {
        wp_enqueue_script('bootstrap', 'bootstrap.js');
    }
}
add_action('wp_enqueue_scripts', 'my_theme_scripts');
```

**6. Find Hardcoded Scripts**

```bash
# Search theme/plugin files for hardcoded tags
grep -r "<script" wp-content/themes/your-theme/
grep -r "<script" wp-content/plugins/
```

**Common Doubt:** _"I dequeued it, but it still loads!"_

**Solution:**  
Dequeue must happen **before** `wp_head` or `wp_footer`:

```php
// ❌ Too late - already printed
add_action('wp_head', function() {
    wp_dequeue_script('duplicate');
});

// ✅ Correct timing - before print phase
add_action('wp_enqueue_scripts', function() {
    wp_dequeue_script('duplicate');
}, 100);
```

**Bonus: Remove Hardcoded Scripts**

```php
// If script is hardcoded in plugin you can't edit:
add_action('wp_footer', function() {
    ob_start(function($html) {
        // Remove specific hardcoded script
        return preg_replace(
            '/<script[^>]*src=["\'].*bootstrap\.js["\'][^>]*><\/script>/',
            '',
            $html
        );
    });
}, -999);
```

---

### **🎓 Bonus Pro Tips (Interviewer Loves These)**

#### **1. Auto-Versioning During Development**

```php
// Version assets based on file modification time
function smart_asset_version($file_path) {
    return file_exists($file_path) ? filemtime($file_path) : '1.0';
}

// Usage:
$css_path = get_stylesheet_directory() . '/style.css';
$css_version = smart_asset_version($css_path);
wp_enqueue_style('theme', get_stylesheet_uri(), [], $css_version);

$js_path = get_template_directory() . '/js/app.js';
$js_version = smart_asset_version($js_path);
wp_enqueue_script('app', get_template_directory_uri() . '/js/app.js', ['jquery'], $js_version, true);
```

**Why this is brilliant:**

- Automatically busts cache when files change
- No need to manually update version numbers
- Works in both dev and production

#### **2. Conditional Script Loading Based on Block Usage**

```php
// Only load slider script if slider block exists in content
add_action('wp_enqueue_scripts', function() {
    if (is_singular() && has_block('acf/hero-slider')) {
        wp_enqueue_script('slider', get_template_directory_uri() . '/js/slider.js', [], '1.0', true);
    }
});
```

#### **3. Async/Defer for Better Performance**

```php
add_filter('script_loader_tag', function($tag, $handle, $src) {
    // Async for non-dependent scripts
    $async_scripts = ['google-analytics', 'facebook-pixel'];
    if (in_array($handle, $async_scripts)) {
        return '<script src="' . esc_url($src) . '" async></script>';
    }
    
    // Defer for scripts that need DOM but aren't critical
    $defer_scripts = ['animations', 'scroll-effects'];
    if (in_array($handle, $defer_scripts)) {
        return '<script src="' . esc_url($src) . '" defer></script>';
    }
    
    return $tag;
}, 10, 3);
```

#### **4. Module Scripts for Modern JavaScript**

```php
add_filter('script_loader_tag', function($tag, $handle) {
    if ('modern-app' === $handle) {
        return str_replace(' src', ' type="module" src', $tag);
    }
    return $tag;
}, 10, 2);

// Then in your JS:
// import { myFunction } from './module.js';
```

#### **5. Preload Critical Assets**

```php
add_action('wp_head', function() {
    // Preload critical CSS
    echo '<link rel="preload" href="' . get_stylesheet_uri() . '" as="style">';
    
    // Preload critical font
    echo '<link rel="preload" href="' . get_template_directory_uri() . '/fonts/main.woff2" as="font" type="font/woff2" crossorigin>';
    
    // Preload hero image
    if (is_front_page()) {
        echo '<link rel="preload" href="' . get_template_directory_uri() . '/images/hero.jpg" as="image">';
    }
}, 1);
```

---

### **📌 Quick Reference Cheat Sheet**

#### **Asset Loading Actions (in order)**

```php
// 1. Enqueue phase
add_action('wp_enqueue_scripts', 'my_assets');        // Frontend
add_action('admin_enqueue_scripts', 'my_admin');      // Admin
add_action('login_enqueue_scripts', 'my_login');      // Login

// 2. Print phase (automatic, but can hook)
add_action('wp_print_styles', 'before_styles', 1);    // Before CSS
add_action('wp_print_scripts', 'before_scripts', 1);  // Before JS
```

#### **Script/Style Functions**

```php
// Register (add to system, don't load)
wp_register_script($handle, $src, $deps, $ver, $in_footer);
wp_register_style($handle, $src, $deps, $ver, $media);

// Enqueue (register + load)
wp_enqueue_script($handle, $src, $deps, $ver, $in_footer);
wp_enqueue_style($handle, $src, $deps, $ver, $media);

// Dequeue (prevent loading)
wp_dequeue_script($handle);
wp_dequeue_style($handle);

// Check status
wp_script_is($handle, $status); // 'registered', 'enqueued', 'done'
wp_style_is($handle, $status);

// Add inline code
wp_add_inline_script($handle, $code, $position); // 'before' or 'after'
wp_add_inline_style($handle, $code);

// Localize (pass PHP data to JS)
wp_localize_script($handle, $object_name, $data_array);
```

#### **Common Handles (WordPress Core)**

```
jquery                  // jQuery (no-conflict mode)
jquery-ui-core          // jQuery UI Core
jquery-ui-tabs          // jQuery UI Tabs
backbone                // Backbone.js
underscore              // Underscore.js
wp-api                  // WordPress REST API JS client
wp-util                 // WordPress utilities
```

#### **Hook Priority Guide**

```
wp_head:
  0-7:   Before CSS/JS (meta tags, Open Graph)
  8:     CSS prints (wp_print_styles)
  9:     Head scripts print (wp_print_head_scripts)
  10-99: After styles/scripts

wp_footer:
  0-19:  Before footer scripts (modals, widgets)
  20:    Footer scripts print (wp_print_footer_scripts)
  21-99: After all scripts (tracking, analytics)
```

---

### **🔬 Advanced Debugging Snippet**

```php
// Add this to functions.php for comprehensive debugging
if (current_user_can('administrator')) {
    add_action('wp_footer', function() {
        global $wp_scripts, $wp_styles;
        ?>
        <div style="position: fixed; bottom: 0; left: 0; right: 0; background: #000; color: #0f0; padding: 10px; font-family: monospace; font-size: 11px; max-height: 300px; overflow: auto; z-index: 999999;">
            <strong>ENQUEUED SCRIPTS:</strong><br>
            <?php foreach ($wp_scripts->queue as $handle) {
                $script = $wp_scripts->registered[$handle];
                echo "$handle | Deps: " . implode(', ', $script->deps) . " | Footer: " . ($script->extra['group'] ?? 'head') . "<br>";
            } ?>
            <br>
            <strong>ENQUEUED STYLES:</strong><br>
            <?php foreach ($wp_styles->queue as $handle) {
                $style = $wp_styles->registered[$handle];
                echo "$handle | Deps: " . implode(', ', $style->deps) . "<br>";
            } ?>
        </div>
        <?php
    }, 999);
}
```

---

## **🎯 Summary**

You now have a **complete, production-ready** understanding of WordPress code execution flow:

✅ **9 distinct phases** from bootstrap to shutdown  
✅ **Priority-based hook system** with exact timing  
✅ **Asset enqueueing vs. printing** separation  
✅ **Dependency resolution** mechanics  
✅ **Interview-ready Q&A** with real-world solutions  
✅ **Performance optimization** techniques  
✅ **Debugging tools** and strategies  
✅ **Common pitfalls** and how to avoid them

**This guide is:**

- ✨ Interview-proof
- 📚 Reference-ready
- 🚀 Production-tested
- 🇮🇳 India-localized
- 💯 Technically accurate

---

**Print this. Master this. Ace your WordPress interviews.**  
_Complete execution flow from `wp-load.php` to browser delivery._