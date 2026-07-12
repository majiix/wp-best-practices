---
name: wp-best-practices
description: "Use when coding, refactoring, or auditing WordPress projects (plugins, themes, or apps). Enforces PHP 7.4+ compatibility, unique prefixing (4+ chars), translation rules, database caching, and specific date/time handling."
---

# WordPress Best Practices and Code Standards

Follow these guidelines for all WordPress-related development tasks.

## Guidelines

### 1. WordPress Coding Standards
- Adhere to the WordPress Coding Standards (WPCS) and the Plugin/Theme Handbooks on developer.wordpress.org.
- Always use standard WordPress coding standards when writing PHP, JavaScript, and TypeScript.

### 2. PHP Compatibility
- The minimum supported PHP version is 7.4.
- All PHP code must be compatible with PHP 7.4 or later.

### 3. Unique Prefixing
- All PHP functions, classes, options, constants, and namespaces must use a unique prefix.
- The prefix must be at least 4 characters long and derived from the name of the plugin, theme, or app (for example: `prefix_` or `sthname_`) to prevent naming collisions.

### 4. Plugin Quality
- Follow the guidelines inside the WordPress Plugin Check plugin.
- Write standard-compliant, secure, and performant code.

### 5. Translation Text Domain
- Always match the text domain to the plugin or theme slug.

### 6. Avoid `strip_tags()`
- The native `strip_tags()` function is discouraged.
- Use the more comprehensive `wp_strip_all_tags()` function instead.

### 7. Do Not Load Plugin Textdomain
- Calling `load_plugin_textdomain()` manually is discouraged (since WordPress 4.6).
- For plugins hosted on WordPress.org, WordPress automatically loads translations under the plugin slug, making manual loading unnecessary.

### 8. Date and Time Handling
- Avoid using the native PHP `date()` function, as it is affected by runtime timezone changes and can result in incorrect date or time displays.
- Use `gmdate()` instead.

### 9. Database Caching
- Do not perform direct database calls without caching.
- Utilize `wp_cache_get()`, `wp_cache_set()`, or `wp_cache_delete()` to cache database queries wherever possible.

### 10. Translation Placeholders
- If using the `__()` translation function with placeholders, always add a `// translators:` comment on the line immediately preceding the call to clarify the meaning of each placeholder.

### 11. Version Updates and Changelog
- When updating any WordPress project (plugin, theme, or app), always update or increment the version number in all relevant files (for example: the main plugin file header, `readme.txt`, or `style.css`).
- If a changelog exists, always document the new changes in it.

### 12. Avoid Global PHP Limit Modifications
- Do not modify PHP execution or memory limits globally (such as calling `set_time_limit()` or `ini_set('memory_limit', ...)` inside constructors, `init` hooks, or global scope).
- If execution limits must be extended, scope them strictly and specifically to only the exact functions that require them (for example, during long-running remote API requests).

### 13. Declare Plugin Dependencies (Requires Plugins)
- If the plugin extends or depends on another plugin in the WordPress.org directory (e.g., WooCommerce, Elementor), always declare it in the main plugin file header using the `Requires Plugins` header.
- The value must be a comma-separated list of WordPress.org plugin slugs (e.g., `Requires Plugins: woocommerce, elementor`).
- Do not add this header to `readme.txt` as WordPress reads it strictly from the main plugin file's header block.

### 14. WordPress.org Reviewer Standards
- **No Inline JS/CSS**: Do not output raw `<script>` or `<style>` HTML tags. Enqueue static JS/CSS via `wp_enqueue_script()` / `wp_enqueue_style()`. For dynamic or localized values, pass them using `wp_localize_script()`.
- **Sanitize Settings Options**: Ensure all options registered through `register_setting()` specify a proper `sanitize_callback`. If the option can contain HTML elements, sanitize it using `wp_kses_post()` or a custom `wp_kses` allowlist instead of returning the input raw or unchanged.
- **REST API Endpoint Authorization**: Always implement strict `permission_callback` arguments for custom REST API routes. Endpoints that write to or modify site-wide settings/options or perform administrative actions must be protected by appropriate admin capabilities (e.g., `current_user_can( 'manage_options' )`) rather than standard post-editing capabilities (like `edit_posts`).
- **No Arbitrary Code/CSS Insertion**: Do not allow users to save or inject custom CSS, JavaScript, or PHP code (e.g., custom CSS textareas). Replicating existing WordPress Customizer/Editor features or allowing arbitrary code fields is prohibited. Use structured setting forms/toggles instead.
- **Disclose External / 3rd Party Services**: If a plugin connects to external APIs/services (e.g., OpenRouter, Stripe, CDN), disclose it clearly in `readme.txt` under a `== External Services ==` section. For each service, document: what the service is, what data is sent and when, and provide direct links to their Terms of Service and Privacy Policy.
- **Strict Unique Prefixing**: All declarations, global variables, stored options, database tables, and public-facing hooks/shortcodes must be prefixed with a unique identifier at least 4 characters long. Avoid using common generic words (such as `ai`, `custom`, `plugin`) as prefixes.

### 15. PHPCS Ignore Annotations & WordPress VIP Rules
- **Combined phpcs:ignore**: Always combine multiple PHPCS ignore targets into a single, comma-separated line (e.g., `// phpcs:ignore WordPress.DB.DirectDatabaseQuery.DirectQuery, WordPress.DB.DirectDatabaseQuery.NoCaching`). Split comments are ignored or apply only to the immediately next line.
- **Single-Line SQL Prepares**: Keep dynamic query shapes and `$wpdb->prepare` calls on a single line if they use PHPCS ignore comments. Multiline layouts trigger line-indexed violations that bypass ignore comments.
- **Input Sanitization Ignorance**: For complex input arrays (like nested `$_POST` structures) where individual element sanitization occurs inside a loop later, use `// phpcs:ignore WordPress.Security.ValidatedSanitizedInput.InputNotSanitized` on the assignment.
- **Table Interpolation**: Database table names cannot be prepared using `%s`. When interpolating table variables (e.g. `FROM $table`), use combined comments to bypass prepared checks.

### 16. Readme Tag Limits
- WordPress.org allows up to 12 tags in `readme.txt`, but only the first 5 are used for search, indexing, and display — anything beyond that is effectively wasted, and anything past 12 is dropped entirely.
- Keep the `Tags` field to 5 well-chosen, relevant terms so nothing meaningful gets ignored.