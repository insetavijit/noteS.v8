
| WP Function / Tag / Feature     | What It Does                                | WP Dev Doc                                                                         | Notes Link                |
| ------------------------------- | ------------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------- |
| `language_attributes()`         | Adds language + text direction to `<html>`. | [Docs](https://developer.wordpress.org/reference/functions/language_attributes/)   | [[language_attributes]]   |
| `bloginfo( 'charset' )`         | Outputs charset (UTF-8).                    | [Docs](https://developer.wordpress.org/reference/functions/bloginfo/)              | [[bloginfo_charset]]      |
| `wp_head()`                     | Outputs WP + plugin scripts, styles, meta.  | [Docs](https://developer.wordpress.org/reference/functions/wp_head/)               | [[wp_head]]               |
| `body_class()`                  | Outputs dynamic classes for `<body>`.       | [Docs](https://developer.wordpress.org/reference/functions/body_class/)            | [[body_class]]            |
| `wp_body_open()`                | Hook fired right after `<body>`.            | [Docs](https://developer.wordpress.org/reference/functions/wp_body_open/)          | [[wp_body_open]]          |
| `esc_html_e()`                  | Echo translated + escaped text.             | [Docs](https://developer.wordpress.org/reference/functions/esc_html_e/)            | [[esc_html_e]]            |
| `the_custom_logo()`             | Displays Customizer logo.                   | [Docs](https://developer.wordpress.org/reference/functions/the_custom_logo/)       | [[the_custom_logo]]       |
| `is_front_page()`               | Checks if static homepage.                  | [Docs](https://developer.wordpress.org/reference/functions/is_front_page/)         | [[is_front_page]]         |
| `is_home()`                     | Checks if blog posts page.                  | [Docs](https://developer.wordpress.org/reference/functions/is_home/)               | [[is_home]]               |
| `esc_url()`                     | Sanitizes URLs.                             | [Docs](https://developer.wordpress.org/reference/functions/esc_url/)               | [[esc_url]]               |
| `home_url()`                    | Returns home URL.                           | [Docs](https://developer.wordpress.org/reference/functions/home_url/)              | [[home_url]]              |
| `bloginfo( 'name' )`            | Site title.                                 | [Docs](https://developer.wordpress.org/reference/functions/bloginfo/)              | [[bloginfo_name]]         |
| `get_bloginfo( 'description' )` | Site tagline.                               | [Docs](https://developer.wordpress.org/reference/functions/get_bloginfo/)          | [[bloginfo_description]]  |
| `is_customize_preview()`        | True in Customizer view.                    | [Docs](https://developer.wordpress.org/reference/functions/is_customize_preview/)  | [[is_customize_preview]]  |
| `wp_nav_menu()`                 | Outputs navigation menu.                    | [Docs](https://developer.wordpress.org/reference/functions/wp_nav_menu/)           | [[wp_nav_menu]]           |
| `__()` / `_e()`                 | Translation functions.                      | [Docs](https://developer.wordpress.org/themes/functionality/internationalization/) | [[translation_functions]] |
| Theme Location: `menu-1`        | Registered menu location.                   | [Docs](https://developer.wordpress.org/reference/functions/register_nav_menus/)    | [[menu_1_location]]       |
| PHPCS Ignore Comment            | Disables WP coding standards warnings.      | [Docs](https://github.com/WordPress/WordPress-Coding-Standards)                    | [[phpcs_ignore]]          |
