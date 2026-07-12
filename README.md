# WordPress Best Practices Skill for AI Coding Agents

A custom skill for AI coding agents (such as Google Antigravity, Claude Code, and other agentic assistants supporting custom rules) designed to enforce modern WordPress coding standards, secure development practices, and compatibility guidelines during development, refactoring, and codebase audits.

## Enforced Standards

This skill configures AI agents to adhere to the latest WordPress standards and review guidelines.

### 1. Compatibility and Modern PHP
* **Minimum PHP 7.4**: Ensures all written or modified code runs seamlessly on PHP 7.4 or newer.
* **Safer Alternatives**: Prefers robust WordPress equivalents over native PHP functions, such as `wp_strip_all_tags()` instead of `strip_tags()`, and `gmdate()` instead of timezone-dependent `date()`.

### 2. Namespace and Naming Collision Prevention
* **Unique Prefixing**: All classes, functions, options, constants, and namespaces must be prefixed with a unique identifier at least 4 characters long (e.g., `prefix_` or `slug_`).

### 3. Security and API Integrity
* **No Inline Scripts or Styles**: Enforces enqueuing static JS/CSS via `wp_enqueue_script()` and `wp_enqueue_style()`.
* **Sanitization of Settings**: All options registered via `register_setting()` must specify a proper `sanitize_callback` using `wp_kses_post()` or similar context-safe sanitizers.
* **REST API Authorization**: Requires strict `permission_callback` checks for custom REST API routes, protecting modifications with appropriate admin capabilities (e.g., `manage_options`).

### 4. Database Optimization
* **Direct Database Query Caching**: Discourages uncached database calls. Requires utilizing `wp_cache_get()` and `wp_cache_set()` where direct queries are necessary.

### 5. Translation Standards
* **Text Domain Alignment**: Automatically matches the text domain to the plugin or theme slug.
* **No Manual Text Domain Loading**: Discourages manual calls to `load_plugin_textdomain()` when hosted on WordPress.org.
* **Translator Comments**: Requires inline translator comments immediately preceding translation functions containing placeholders (e.g., `// translators: ...`).

## Installation

You can install this skill either globally for all projects or locally within a specific project repository.

### Global Installation

Clone this repository directly into your agent's global skills configuration directory. For example:

* **Google Antigravity**:
  ```bash
  git clone https://github.com/majiix/wp-best-practices.git "%USERPROFILE%\.gemini\config\skills\wp-best-practices"
  ```
* **Claude Code**:
  ```bash
  git clone https://github.com/majiix/wp-best-practices.git "%USERPROFILE%\.claude\config\skills\wp-best-practices"
  ```

### Workspace-Specific Installation

To activate the skill for a single project:
1. Create an `.agents/skills` directory at the root of your WordPress project repository if it does not exist.
2. Clone this repository into that directory:
   ```bash
   git clone https://github.com/majiix/wp-best-practices.git .agents/skills/wp-best-practices
   ```

## Configuration

The core instructions are located in the [SKILL.md](SKILL.md) file. The frontmatter defines the trigger and activation rules:

```yaml
---
name: wp-best-practices
description: "Use when coding, refactoring, or auditing WordPress projects (plugins, themes, or apps). Enforces PHP 7.4+ compatibility, unique prefixing (4+ chars), translation rules, database caching, and specific date/time handling."
---
```

## Contributing

Contributions to refine or expand these best practices are welcome. Please open an issue or submit a pull request with your suggested improvements.

## License

This project is licensed under the MIT License.
