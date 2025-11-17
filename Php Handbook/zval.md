# PHP `zval` – Internal Structure Reference

> **Type:** Zend Engine Internal | Memory Management  
> **Level:** Advanced / Extension Development  
> **Updated:** November 17, 2025  
> **Related:** Copy-on-Write, Reference Counting, PHP 7+ Optimizations

---

## What is a `zval`?

**Zend Value** – The fundamental internal structure the PHP engine uses to store every variable's value and metadata.

|Aspect|Description|
|---|---|
|**Full Name**|Zend Value|
|**Location**|Inside Zend Engine (PHP's C core)|
|**Visibility**|Internal only – never exposed to PHP code|
|**Purpose**|Enables dynamic typing, reference counting, copy-on-write, and memory optimization|
|**Since**|PHP 4.0 (restructured in PHP 7.0)|

---

## Core Components

Every `zval` contains four essential pieces of information:

|Field|Type|Purpose|Example|
|---|---|---|---|
|**value**|`zvalue_value`|Stores actual data (int, string pointer, array pointer, etc.)|`42`, `"hello"`, `0x7f8a3c`|
|**type**|`zend_uchar`|PHP type tag|`IS_LONG`, `IS_STRING`, `IS_ARRAY`, `IS_OBJECT`|
|**refcount**|`zend_uint`|How many variables reference this zval|`1`, `2`, `5`|
|**is_ref**|`zend_uchar`|Whether this is a PHP reference (`&`)|`0` (false), `1` (true)|

---

## Mental Model: Variables → zvals

### Basic Assignment

```php
$a = 10;
```

**Internally:**

```
Variable Symbol Table:
  "a" → zval {
    value: 10,
    type: IS_LONG,
    refcount: 1,
    is_ref: 0
  }
```

### Copy-on-Write (Multiple Variables, Same zval)

```php
$a = 10;
$b = $a;  // No copy yet, just share the zval
```

**Internally:**

```
Symbol Table:
  "a" → zval_1 {value: 10, type: IS_LONG, refcount: 2, is_ref: 0}
  "b" → zval_1 (same pointer)
```

### Copy-on-Write Triggered

```php
$a = 10;
$b = $a;
$b = 20;  // NOW PHP copies the zval
```

**Internally:**

```
Before $b = 20:
  "a" → zval_1 {value: 10, refcount: 2}
  "b" → zval_1

After $b = 20:
  "a" → zval_1 {value: 10, refcount: 1}
  "b" → zval_2 {value: 20, refcount: 1}  ← New zval created
```

### References (`&`)

```php
$a = 10;
$b = &$a;  // True reference
$b = 20;   // $a also becomes 20
```

**Internally:**

```
Symbol Table:
  "a" → zval_1 {value: 20, type: IS_LONG, refcount: 2, is_ref: 1}
  "b" → zval_1 (same pointer, is_ref=1)
```

---

## C Structure (PHP 5.x)

```c
typedef struct _zval_struct {
    zvalue_value value;        // Union holding actual data
    zend_uint refcount__gc;    // Reference counter
    zend_uchar type;           // Type tag (IS_LONG, IS_STRING, etc.)
    zend_uchar is_ref__gc;     // Is this a reference?
} zval;
```

### The `zvalue_value` Union

```c
typedef union _zvalue_value {
    long lval;                 // IS_LONG
    double dval;               // IS_DOUBLE
    struct {                   // IS_STRING
        char *val;
        int len;
    } str;
    HashTable *ht;             // IS_ARRAY
    zend_object_value obj;     // IS_OBJECT
} zvalue_value;
```

---

## PHP 7+ Changes (Major Restructure)

PHP 7 introduced a **radically different** zval design for massive performance gains:

|PHP 5.x|PHP 7+|Impact|
|---|---|---|
|zval = 48 bytes|zval = 16 bytes|**67% smaller**|
|Heap-allocated|Stack/embedded|Fewer mallocs|
|Separate refcount|Embedded in strings/arrays|Faster|
|Always boxed|Inline scalars|No pointers for ints/floats|

### PHP 7+ Structure (Simplified)

```c
struct _zval_struct {
    zend_value value;          // 8 bytes
    union {
        uint32_t type_info;    // 4 bytes (type + flags)
    } u1;
    union {
        uint32_t next;         // 4 bytes (for hashtables)
    } u2;
};
```

**Key PHP 7 Optimization:**

```php
// PHP 5: Two heap allocations (zval + refcount)
$a = 10;

// PHP 7: Zero heap allocations (zval on stack, int inline)
$a = 10;
```

---

## Copy-on-Write (COW) Explained

PHP delays copying data until absolutely necessary.

### Without COW (Hypothetical)

```php
$a = "large string";
$b = $a;  // ❌ Immediate copy = waste memory
```

### With COW (PHP Reality)

```php
$a = "large string";
$b = $a;              // ✅ Share zval, refcount++
$b = "new string";    // ✅ NOW copy (COW triggered)
```

**Benefits:**

- Saves memory when variables aren't modified
- Faster assignments
- Automatic optimization (you don't control it)

---

## Reference Counting Lifecycle

```php
$a = "hello";     // zval created, refcount=1

$b = $a;          // Share zval, refcount=2

unset($a);        // refcount=1 (still alive for $b)

unset($b);        // refcount=0 → zval destroyed
```

**Internal Flow:**

```
1. zval_alloc()      → Create zval, refcount=1
2. $b = $a           → Z_ADDREF(zval), refcount=2
3. unset($a)         → Z_DELREF(zval), refcount=1
4. unset($b)         → Z_DELREF(zval), refcount=0
5. refcount=0        → zval_ptr_dtor() → free memory
```

---

## Why Developers Should Care

|Scenario|Relevance|What You Need to Know|
|---|---|---|
|**Memory optimization**|High|PHP automatically shares zvals until writes occur|
|**References (`&$var`)**|Medium|Changes `is_ref` flag; prevents COW optimizations|
|**Array/object copying**|High|Arrays COW; objects always share (PHP 5+)|
|**Extension development**|Critical|Must manually manage zval refcounts in C code|
|**Performance tuning**|Medium|Understand when PHP copies vs shares data|
|**Debugging memory leaks**|High|Refcount issues cause memory leaks in extensions|

---

## Common Patterns & Implications

### Pattern 1: Array COW

```php
$arr1 = range(1, 1000000);  // 1M elements
$arr2 = $arr1;              // No copy, shares zval
$arr2[] = 99;               // NOW copies entire array
```

**Takeaway:** Large array assignments are cheap until modification.

---

### Pattern 2: References Break COW

```php
$a = "string";
$b = $a;        // COW active, shared zval

$c = &$a;       // Breaks COW, $a now is_ref=1
$b = "new";     // $b gets new zval (cannot share with ref)
```

**Takeaway:** References prevent memory sharing optimizations.

---

### Pattern 3: Objects Never COW

```php
class User { public $name = "Ram"; }

$obj1 = new User();
$obj2 = $obj1;       // Object handle shared (not copied)
$obj2->name = "Sita"; // Affects $obj1 too!

echo $obj1->name;    // "Sita"
```

**Takeaway:** Objects are always references (since PHP 5).

---

### Pattern 4: Strings in PHP 7+

```php
// PHP 5: String zval on heap
$str = "hello";  // malloc() for zval + string

// PHP 7: Interned strings (literal strings reused)
$str = "hello";  // No allocation if "hello" already exists
```

---

## Debugging zvals (Extension Code)

### Check Refcount

```c
zval *my_var;
php_printf("Refcount: %d\n", Z_REFCOUNT_P(my_var));
```

### Check Type

```c
if (Z_TYPE_P(my_var) == IS_STRING) {
    php_printf("String value: %s\n", Z_STRVAL_P(my_var));
}
```

### Increment Refcount (Avoid Memory Loss)

```c
Z_ADDREF_P(my_var);  // Before storing zval elsewhere
```

### Decrement Refcount (Free When Done)

```c
Z_DELREF_P(my_var);
if (Z_REFCOUNT_P(my_var) == 0) {
    zval_ptr_dtor(&my_var);
}
```

---

## Memory Diagram

### PHP 5 zval Layout (48 bytes)

```
┌─────────────────────────────────────────┐
│ zval structure (heap allocated)         │
├─────────────────────────────────────────┤
│ value (union) - 16 bytes                │
│   ├─ lval (long)                        │
│   ├─ dval (double)                      │
│   ├─ str (char* + len)                  │
│   ├─ ht (HashTable*)                    │
│   └─ obj (zend_object_value)            │
├─────────────────────────────────────────┤
│ refcount__gc - 4 bytes                  │
├─────────────────────────────────────────┤
│ type - 1 byte (IS_LONG, IS_STRING, ...) │
├─────────────────────────────────────────┤
│ is_ref__gc - 1 byte (0 or 1)            │
└─────────────────────────────────────────┘
```

### PHP 7+ zval Layout (16 bytes)

```
┌──────────────────────────────────────┐
│ zval (stack or embedded)             │
├──────────────────────────────────────┤
│ value (8 bytes)                      │
│   ├─ Scalar: inline value            │
│   └─ Complex: pointer to data        │
├──────────────────────────────────────┤
│ u1.type_info (4 bytes)               │
│   ├─ type (8 bits)                   │
│   ├─ flags (24 bits)                 │
├──────────────────────────────────────┤
│ u2 (4 bytes) - multipurpose          │
└──────────────────────────────────────┘
```

---

## Type Constants

|Constant|Value|PHP Type|Storage|
|---|---|---|---|
|`IS_UNDEF`|0|Undefined|-|
|`IS_NULL`|1|`null`|No data|
|`IS_FALSE`|2|`false`|No data (PHP 7+)|
|`IS_TRUE`|3|`true`|No data (PHP 7+)|
|`IS_LONG`|4|`int`|`value.lval`|
|`IS_DOUBLE`|5|`float`|`value.dval`|
|`IS_STRING`|6|`string`|`value.str`|
|`IS_ARRAY`|7|`array`|`value.arr`|
|`IS_OBJECT`|8|`object`|`value.obj`|
|`IS_RESOURCE`|9|`resource`|`value.res`|
|`IS_REFERENCE`|10|`&$var`|`value.ref`|

---

## Interview Questions

**Q: What happens when you assign one variable to another?**

A: PHP uses copy-on-write. Both variables share the same zval (refcount increases). A copy only occurs when one variable is modified.

```php
$a = [1, 2, 3];  // zval_1, refcount=1
$b = $a;         // zval_1, refcount=2 (no copy)
$b[] = 4;        // zval_2 created, $b gets new zval
```

---

**Q: How do references differ from regular assignments?**

A: References set `is_ref=1` and prevent copy-on-write. Changes to one variable immediately affect the other.

```php
$a = 10;
$b = &$a;   // is_ref=1, refcount=2, same zval
$b = 20;    // $a becomes 20 (no copy)
```

---

**Q: Why are objects always passed "by reference"?**

A: Since PHP 5, object variables hold **object handles**, not the object itself. Assignment copies the handle (zval), but both handles point to the same object.

```php
$obj1 = new stdClass();
$obj2 = $obj1;  // Handle copied, object shared
```

---

**Q: What changed with zvals in PHP 7?**

A: Size reduced from 48 to 16 bytes, scalars stored inline (no pointers), refcounting moved to strings/arrays, zvals embedded in data structures instead of heap-allocated.

**Impact:** ~2x speed boost, ~50% memory reduction.

---

**Q: When does PHP actually copy data?**

A: Only when:

1. A shared zval is modified (COW triggered)
2. A reference (`&`) prevents sharing
3. Explicitly using `clone` for objects

---

## Extension Development Essentials

### Creating a zval

```c
zval *my_var;
MAKE_STD_ZVAL(my_var);  // PHP 5
ZVAL_STRING(my_var, "hello", 1);

// PHP 7+
zval my_var;
ZVAL_STRING(&my_var, "hello");
```

### Returning a zval

```c
PHP_FUNCTION(my_function)
{
    RETURN_STRING("hello");  // Automatically handles zval
}
```

### Common Mistakes

```c
// ❌ WRONG: Memory leak
zval *tmp;
MAKE_STD_ZVAL(tmp);
// ... forget to zval_ptr_dtor(&tmp);

// ✅ CORRECT
zval *tmp;
MAKE_STD_ZVAL(tmp);
ZVAL_STRING(tmp, "test", 1);
// ... use tmp ...
zval_ptr_dtor(&tmp);  // Free when done
```

---

## Performance Insights

### Memory Usage

```php
// PHP 5: ~48 bytes per variable
$a = 10;  // zval on heap = 48 bytes

// PHP 7: ~0 bytes (stack allocated)
$a = 10;  // Inline storage
```

### COW Efficiency

```php
// Efficient: No copies until write
$arr = range(1, 1000000);
for ($i = 0; $i < 100; $i++) {
    $copy = $arr;  // Just refcount++
}

// Inefficient: Forces copies
$arr = range(1, 1000000);
for ($i = 0; $i < 100; $i++) {
    $copy = $arr;
    $copy[] = $i;  // Copy entire array 100 times
}
```

---

## Resources

- **PHP Internals Book:** [Zvals](https://www.phpinternalsbook.com/php7/internal_types/zvals.html)
- **Source Code:** `Zend/zend_types.h` (zval definition)
- **Nikita Popov (PHP Core):** [PHP 7 zval Changes](https://nikic.github.io/2015/05/05/Internal-value-representation-in-PHP-7-part-1.html)
- **PHP Manual:** [References Explained](https://www.php.net/manual/en/language.references.php)

---

## Quick Reference

```c
// PHP 5
typedef struct _zval_struct {
    zvalue_value value;
    zend_uint refcount__gc;
    zend_uchar type;
    zend_uchar is_ref__gc;
} zval;

// PHP 7+
struct _zval_struct {
    zend_value value;        // 8 bytes
    union { uint32_t type_info; } u1;  // 4 bytes
    union { uint32_t next; } u2;       // 4 bytes
};

// Common Macros
Z_TYPE_P(zval_ptr)      // Get type
Z_LVAL_P(zval_ptr)      // Get long value
Z_STRVAL_P(zval_ptr)    // Get string value
Z_REFCOUNT_P(zval_ptr)  // Get refcount
Z_ADDREF_P(zval_ptr)    // Increment refcount
Z_DELREF_P(zval_ptr)    // Decrement refcount
```

---

**Summary:** `zval` is PHP's internal variable container enabling dynamic typing, memory optimization through COW, and reference counting. PHP 7 revolutionized zval design for massive performance gains. Understanding zvals is essential for extension development and advanced optimization.

**Relevance:** ★★★★★ (Extension Devs) | ★★★☆☆ (App Devs)  
**Complexity:** Advanced  
**PHP Versions:** All (structure changed in PHP 7)