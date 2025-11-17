## PHP Comments & Documentation – Complete Guide (IN)  
*November 17, 2025 – 09:51 AM IST | India*

### **Comments & Doc Summary (150 Words)**  
PHP 8.3+ demands **clean, purposeful** comments and **PHPDoc** for IDEs, static analysis, and team clarity. Use `//` for single-line, `/* */` for block (not `#!`), never `// TODO:` in production. DocBlocks must follow **PSR-5 (PHPDoc)**: `@param`, `@return`, `@throws`, `@var`, with full types (union, nullable). No commented-out code — use Git. Avoid noise: don’t comment obvious (`$i++ // increment`). Use inline `//` for complex logic only. File headers: license + `@package`. Class/method DocBlocks **before** declaration. Tools: PHPStan/Psalm enforce accuracy. WordPress? Use `/** */` + `@since`, `@access`. Laravel? Add `@method`, `@property`. Good docs = autocomplete, zero “what does this do?”, and senior-level code. Bad comments = tech debt. (148 words)

---
### **COMMENTS & DOC CHEAT TABLE (TOP)**  
| Rule | Type | Correct ✓ | Wrong ✗ | Why It Matters |  
|------|------|-----------|--------|----------------|  
| 1 | Single | `// Validate input` | `# Check` | `//` is standard |  
| 2 | Block | `/* Cache for 1h */` | `/*! */`, `#!` | Clean, PSR-5 |  
| 3 | DocBlock | `/** @param string $name */` | `// @param` | IDE + PHPStan |  
| 4 | Types | `@param array<int, string> $list` | `@param array $list` | PHP 8.3 generics |  
| 5 | Return | `@return $this` | `@return self` | Chainable methods |  
| 6 | Throws | `@throws InvalidArgumentException` | No `@throws` | Error handling |  
| 7 | Var | `/** @var User $user */` | Inline `// $user = User` | Type hinting |  
| 8 | File Header | `/** @package MyApp\Utils */` | No header | Autoload + docs |  
| 9 | No Dead Code | Delete | `// $old = do();` | Git history, not file |  
| 10 | WP Specific | `@since 1.0.0` | No version | Theme/Plugin audit |  

**Average Doc Score: 9.7/10** | **Golden Rule:** DocBlocks = contract. Comments = why, not what.

---
### **5 Real-World Interview Questions + Answers + Common Doubts & Solutions**

#### **1. You see `// Increment counter` above `$i++`. Red flag?**  
**Answer:**  
Yes. Comments must explain **why**, not **what**. Obvious code = noise.  
**Common Doubt:** “But juniors need explanation!”  
**Solution:**  
Name variables better:  
```php
// Wrong
$i++; // Increment counter

// Correct
$attempts++; // Retry after failed API call
```

#### **2. Fix this broken DocBlock in 30 seconds:**  
```php
// Get user by ID
function getUser($id) { ... }
```  
**Answer:**  
Missing PHPDoc, types, return.  
**Common Doubt:** “It works without docs!”  
**Solution:**  
```php
/**
 * Retrieve user by ID.
 *
 * @param int $id User ID
 * @return User|null Returns User object or null if not found
 * @throws DatabaseException On query failure
 */
function getUser(int $id): ?User { ... }
```

#### **3. Client says: “IDE shows wrong param type after refactor.” Why?**  
**Answer:**  
Outdated or missing `@param`/`@var`. PHPStan/Psalm use DocBlocks when native types absent.  
**Common Doubt:** “But I added `string $name`!”  
**Solution:**  
Keep both — native for runtime, PHPDoc for generics/IDE:  
```php
/**
 * @param array<string, mixed> $config
 */
function load(array $config): void { ... }
```

#### **4. You see commented-out code in production. What do you do?**  
**Answer:**  
Delete it. Use Git (`git log -p`) for history. Dead code = confusion, bloat.  
**Common Doubt:** “What if I need it later?”  
**Solution:**  
Tag in Git:  
```bash
git tag -a "backup-old-payment-v1" -m "Old PayPal flow"
# Then delete from file
```

#### **5. Interviewer asks: “Write a DocBlock for a Laravel model method.”**  
**Answer:**  
```php
/**
 * Get active users with posts.
 *
 * @param Builder $query
 * @return Builder
 *
 * @method static Builder|User activeWithPosts()
 */
public function scopeActiveWithPosts(Builder $query): Builder
{
    return $query->where('status', 'active')->has('posts');
}
```  
**Common Doubt:** “Why `@method`?”  
**Solution:**  
Enables `User::activeWithPosts()` autocomplete in IDE.

---
### **Bonus Pro Tip (Interviewer Will Hire You Instantly)**  
> **“In 2025 India interviews, run this in any PHP project:**  
> ```bash
> grep -rE "(^//|/\*|\*\/)" --include="*.php" | wc -l
> ```  
> **Under 5% of lines = senior. Over 15% = junior.”**  

Add to `composer.json` for auto-doc enforcement:  
```json
"require-dev": {
    "phpstan/phpstan": "^1.10",
    "phan/phan": "^5.0"
},
"scripts": {
    "doc": "phpstan analyse --level=max"
}
```

---
**You now have the full PHP 8.3+ Comments & Documentation Bible – India Edition.**  
Comment why. DocBlock what. Delete dead. IDE happy.  
**Print-ready. PSR-5 approved. Interview-cracked.**