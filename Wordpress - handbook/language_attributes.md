| **Left Column**       | **Right Column**                                                                                                                                              |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Function**          | `language_attributes()`                                                                                                                                       |
| **Used In**           | Usually inside the `<html>` tag of WordPress themes.                                                                                                          |
| **Purpose**           | Outputs the correct **language** and **text direction** attributes (e.g., `lang="en-US" dir="ltr"`).                                                          |
| **Why It Exists**     | Ensures browsers and assistive tech know the page language → improves **SEO**, **accessibility**, **screen readers**, and **internationalization**.           |
| **Output Example**    | `<html lang="en-US" dir="ltr">`                                                                                                                               |
| **Determined From**   | Site language setting in **WP Admin → Settings → General → Site Language**.                                                                                   |
| **LTR vs RTL**        | If the chosen language is RTL (Arabic, Hebrew, Urdu), it automatically outputs: `dir="rtl"`.                                                                  |
| **Does NOT Affect**   | It **does not translate content**. Posts written in English stay in English. It only sets the HTML `lang` & `dir` attributes, nothing else.                   |
| **Usage**             | Inside theme header: `<html <?php language_attributes(); ?>>`                                                                                                 |
| **Returns vs Echoes** | It **echoes** by default. Use `get_language_attributes()` to **return** instead of print.                                                                     |
| **Custom Filters**    | You can extend attributes using the `language_attributes` filter (e.g., add Open Graph prefix: `<html prefix="og: http://ogp.me/ns#">`).                      |
