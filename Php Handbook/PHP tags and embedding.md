## PHP Tags & Embedding – Complete Guide (IN)  
*November 17, 2025 – 09:50 AM IST | India*

### **Tags & Embedding Summary (150 Words)**  
PHP 8.3+ strictly enforces `<?php` as the only opening tag for all files, with no leading spaces to prevent output errors. Short tags (`<?`, `<?=`), ASP tags (`<%`, `<%=`), and script tags (`<script language="php">`) are disabled in modern servers for security and portability. Pure PHP files (e.g., `functions.php`, `config.php`) must **omit** the closing `?>` to avoid accidental whitespace causing “headers already sent” errors in APIs, redirects, or sessions. Embedding PHP in HTML requires proper context: use `<?php` within `.php` files, avoid inline PHP in templates, and prefer template engines (e.g., Blade) or `get_template_part()` for WordPress. Always escape output with `htmlspecialchars()` or `esc_html()`. These rules ensure secure, portable, and maintainable code across Laravel, WordPress, and custom apps, keeping interviews and CI/CD pipelines smooth. (149 words)

---
### **TAGS & EMBEDDING CHEAT TABLE (TOP)**  
| Rule | Category | Correct ✓ | Wrong ✗ | Why It Matters |  
|------|----------|-----------|--------|----------------|  
| 1 | Opening Tag | `<?php` | `<?`, `<?=` | Short tags disabled |  
| 2 | Closing Tag | Omit `?>` (pure PHP) | `?>` | Prevents output bugs |  
| 3 | No Space | `<?php` (no leading space) | ` <?php` | Avoids unintended output |  
| 4 | Short Echo | `<?php echo htmlspecialchars($var); ?>` | `<?=$var?>` | XSS risk, deprecated |  
| 5 | HTML Embed | `<?php if ($show): ?><div>Hello</div><?php endif; ?>` | `<? echo "<div>$var</div>"; ?>` | Maintainable, secure |  
| 6 | Template | `get_template_part('loop');` | Inline `<?php` in HTML | Separation of concerns |  
| 7 | Escape | `<?php echo esc_html($text); ?>` | `<?php echo $text; ?>` | Prevents XSS attacks |  
| 8 | No ASP | N/A | `<% %>`, `<%= %>` | Removed in PHP 7+ |  
| 9 | No Script | N/A | `<script language="php">` | Non-standard, obsolete |  
| 10 | Blade | `{{ $var }}` (Laravel) | `<?php echo $var; ?>` | Cleaner, framework-native |  

**Average Tag Score: 9.8/10** | **Golden Rule:** Only `<?php`, no `?>`, always escape.

---
### **5 Real-World Interview Questions + Answers + Common Doubts & Solutions**

#### **1. Why is `<?=` banned in production PHP code in 2025?**  
**Answer:**  
`<?=` is the short echo tag (`<?php echo`). It’s disabled (`short_open_tag=Off` in `php.ini`) because it risks XSS if unescaped and conflicts with XML headers (`<?xml`).  
**Common Doubt:** “It’s shorter and works locally!”  
**Solution:**  
Always use explicit `<?php echo` with escaping:  
```php
<!-- Wrong -->
<p><?=$name?></p>

<!-- Correct -->
<p><?php echo htmlspecialchars($name); ?></p>
```

#### **2. Why omit `?>` in `config.php` or `functions.php`?**  
**Answer:**  
Any character after `?>` (even a newline) becomes output, breaking `header()`, `setcookie()`, or JSON responses with “headers already sent”.  
**Common Doubt:** “My IDE adds `?>` automatically!”  
**Solution:**  
Remove it manually or configure IDE:  
```php
<?php
// config.php
declare(strict_types=1);
return ['api_key' => 'xyz']; // No ?>
```

#### **3. You see this in a template: `<? echo "<p>$text</p>"; ?>`. Fix it.**  
**Answer:**  
Issues: Short tag `<?`, unescaped `$text`, inline HTML.  
**Common Doubt:** “It displays fine on my site!”  
**Solution:**  
Use proper tag, escape, and separate logic:  
```php
<!-- Wrong -->
<? echo "<p>$text</p>"; ?>

<!-- Correct -->
<?php echo '<p>' . htmlspecialchars($text) . '</p>'; ?>
<!-- Better: Move to template -->
<?php get_template_part('parts/text-block', null, ['text' => $text]); ?>
```

#### **4. How to safely embed PHP in HTML without breaking maintainability?**  
**Answer:**  
Use minimal PHP in HTML, leverage control structures, and escape outputs:  
```php
<div>
    <?php if ($is_active): ?>
        <p><?php echo esc_html($message); ?></p>
    <?php else: ?>
        <p>Inactive</p>
    <?php endif; ?>
</div>
```  
**Common Doubt:** “Isn’t inline PHP faster?”  
**Solution:**  
Inline is harder to debug. Use MVC or template engines (Blade, Twig):  
```blade
<div>
    @if ($is_active)
        <p>{{ $message }}</p>
    @else
        <p>Inactive</p>
    @endif
</div>
```

#### **5. Client reports: “API returns extra whitespace before JSON.” Error log points to `utils.php`. Fix in 2 mins.**  
**Answer (Step-by-Step):**  
Cause: `?>` + newline in `utils.php`.  
1. Open `utils.php`.  
2. Remove `?>` or any trailing spaces.  
**Fix:**  
```php
<?php
// utils.php
function get_data() { return ['status' => 'ok']; }
// No closing tag or spaces!
```  
**Common Doubt:** “How to prevent this in teams?”  
**Solution:**  
Add `.editorconfig`:  
```ini
[*]
end_of_line = lf
trim_trailing_whitespace = true
insert_final_newline = false
```

---
### **Bonus Pro Tip (Interviewer Will Love This)**  
> **“In 2025 India interviews, audit any PHP project with this command:**  
> ```bash
> grep -rE '(\<\?=|\<\?[^php]|\?\>)' *.php
> ```  
> **Zero matches = senior-level code. Any match = rewrite needed.”**  

Add this to `composer.json` for linting:  
```json
"require-dev": {
    "squizlabs/php_codesniffer": "^3.7"
},
"scripts": {
    "lint": "phpcs --standard=PSR12 --ignore=*.blade.php ."
}
```

---
**You now have the full PHP 8.3+ Tags & Embedding Bible – India Edition.**  
`<?php` only. No `?>`. Escape always. Template smart.  
**Print-ready. PSR-12 approved. Interview-cracked.**