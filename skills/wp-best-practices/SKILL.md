---
name: wp-best-practices
description: "Use when coding, refactoring, or auditing WordPress projects (plugins, themes, or apps). Enforces PHP 7.4+ compatibility, unique prefixing (4+ chars), translation rules, database caching, specific date/time handling, and high-traffic/concurrency performance rules (REST endpoints, session handling, request batching)."
---

# WordPress Best Practices and Code Standards

Follow these guidelines for all WordPress-related development tasks.

## Guidelines

### 1. WordPress Standards and Codex
- Adhere to the best practices in the most recent WordPress Codex.
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
- **No Arbitrary Code/CSS Insertion**: Do not allow users to save or inject custom CSS, JavaScript, or PHP code (e.g., custom CSS textareas). Replicate existing WordPress Customizer/Editor features or allow arbitrary code fields is prohibited. Use structured setting forms/toggles instead.
- **Disclose External / 3rd Party Services**: If a plugin connects to external APIs/services (e.g., OpenRouter, Stripe, CDN), disclose it clearly in `readme.txt` under a `== External Services ==` section. For each service, document: what the service is, what data is sent and when, and provide direct links to their Terms of Service and Privacy Policy.
- **Strict Unique Prefixing**: All declarations, global variables, stored options, database tables, and public-facing hooks/shortcodes must be prefixed with a unique identifier at least 4 characters long. Avoid using common generic words (such as `ai`, `custom`, `plugin`) as prefixes.

### 15. PHPCS Ignore Annotations & WordPress VIP Rules
- **Combined phpcs:ignore**: Always combine multiple PHPCS ignore targets into a single, comma-separated line (e.g., `// phpcs:ignore WordPress.DB.DirectDatabaseQuery.DirectQuery, WordPress.DB.DirectDatabaseQuery.NoCaching`). Split comments are ignored or apply only to the immediately next line.
- **Single-Line SQL Prepares**: Keep dynamic query shapes and `$wpdb->prepare` calls on a single line if they use PHPCS ignore comments. Multiline layouts trigger line-indexed violations that bypass ignore comments.
- **Input Sanitization Ignorance**: For complex input arrays (like nested `$_POST` structures) where individual element sanitization occurs inside a loop later, use `// phpcs:ignore WordPress.Security.ValidatedSanitizedInput.InputNotSanitized` on the assignment.
- **Table Interpolation**: Database table names cannot be prepared using `%s`. When interpolating table variables (e.g. `FROM $table`), use combined comments to bypass prepared checks.

### 16. Readme Tag Limits
- **Strict Tag Limits**: Limit tags list in `readme.txt` to a maximum of 5 tags to prevent WordPress.org repository rejection.

### 17. Human-Readable Code (No Obfuscation)
- **No Obfuscation**: Code must be human-readable. Do not use packers, minifiers without providing source, or unclear naming conventions (such as `$z12sdf813d`). Provide public access to source code and build tools.

### 18. No Trialware
- **No Trialware or Locked Features**: Plugins must not restrict or lock functionality inside the distributed code that can only be unlocked by payment. Premium features should be separated into add-on plugins hosted outside of WordPress.org.

### 19. Opt-In User Tracking
- **Opt-In Consent**: Do not contact external servers or collect user data without explicit, authorized opt-in consent from the user. Document all data collection clearly.

### 20. No External Executable Code or CDN Assets
- **Local Resources**: All non-service JS and CSS must be included locally. Do not load external executable code, do not load scripts or styles from CDNs (web font inclusions are allowed), and do not use iframes to connect admin pages.

### 21. Opt-In Credit Links
- **Default Off for Credits**: Any "Powered By" or credit links must be optional, default to off, and require explicit user opt-in.

### 22. Non-Intrusive Admin Notices
- **Dismissible Notices**: Avoid hijacking the admin dashboard. Upgrade prompts, notices, and alerts must be limited in scope, and site-wide notices or widget embeds must be dismissible or self-dismiss when resolved.

### 23. Direct Affiliate Links
- **No Cloaked Links**: Affiliate links in the readme or public pages must be disclosed, and they must link directly to the service without redirects or cloaking.

### 24. Use WordPress Default Libraries
- **No Duplicate Libraries**: Do not bundle libraries that are already packaged with WordPress (such as jQuery, PHPMailer, simplepie, etc.). Use the WordPress-packaged versions instead.

### 25. Respect Trademarks in Naming
- **Trademark Rules**: Do not start a plugin name or slug with trademarked terms (e.g., use "for WooCommerce" instead of starting the name/slug with "WooCommerce").


## Performance Rules for High-Traffic WordPress/WooCommerce Plugins

Apply these rules to every plugin you write, regardless of its purpose or feature set:

### 26. Endpoints
- Use a dedicated REST API endpoint (`register_rest_route`) instead of `admin-ajax.php`.
- Each endpoint should execute only the logic it strictly needs — don't route through unnecessary WordPress hooks or lifecycle layers.

### 27. Session & State
- Implement session/cache handling to support both backends: detect whether Redis/Object Cache is active, and use it when available; fall back to MySQL when it isn't.
- Avoid unnecessary reads/writes to session or the database on every request — don't rewrite data that hasn't changed, regardless of backend.
- Avoid any fragment-refresh or full state-rebuild pattern after small operations; return only the piece of state that actually changed in the response.

### 28. Network Requests
- Each user action (add/remove/update) should trigger exactly one network request, not a chain of several.
- Disable buttons client-side during an in-flight request (debounce) to prevent duplicate submissions.
- Implement error/timeout states and retry in the UI — the user should never think the site has frozen.

### 29. Caching
- Never cache output from endpoints that is personalized (nonce-bound, user-specific).
- Keep public-facing pages (product, listing) full-page-cache friendly; keep all dynamic logic inside the dedicated endpoint, not in page rendering.

### 30. Security Without Overhead
- Don't strip out nonce/validation, but avoid triggering unnecessary WordPress hooks along the request path.

### 31. Code Architecture
- Each plugin: a separate REST controller + a separate helper/service layer.
- Keep database/state logic fully decoupled from UI rendering logic (JS/modal/forms).

### 32. Conditional Asset Loading
- Never enqueue JS/CSS site-wide via `wp_enqueue_scripts` without a condition. Check `is_product()`, `is_cart()`, a shortcode presence, etc., and load assets only on the pages that actually need them.

### 33. Autoloaded Options
- When storing options via `add_option()`/`update_option()`, set `autoload => false` for large or infrequently-read values. Large autoloaded options get pulled into memory on every single page load, even when unused.

### 34. Avoid Blocking External Requests on the Frontend
- Never call `wp_remote_get()`/`wp_remote_post()` synchronously during frontend page render. If an external API call is required, either cache the result with a transient/object cache, or move it to an async/background job (e.g. Action Scheduler, WP-Cron with a reasonable interval).
- Always set an explicit `timeout` on any remote request.

### 35. WP-Cron on High-Traffic Sites
- Do not rely on WordPress's default pseudo-cron (triggered by page visits) for scheduled tasks on high-traffic sites — it adds overhead to random requests and is unreliable under load.
- Suggest adding `DISABLE_WP_CRON` in wp-config and recommend a real system cron job instead.

### 36. Query Efficiency
- Avoid unbounded or unindexed `meta_query`/`tax_query` on custom queries; add explicit indexes on custom tables for any column used in a `WHERE` or `JOIN`.
- Avoid `posts_per_page => -1` on high-traffic queries; always paginate.
- Batch bulk operations (e.g. bulk inserts/updates) instead of looping single queries.