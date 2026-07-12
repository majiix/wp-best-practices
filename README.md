# WordPress Best Practices Skill for AI Coding Agents

A custom skill for AI coding agents (such as Google Antigravity, Claude Code, and other agentic assistants supporting custom rules) designed to enforce modern WordPress coding standards, secure development practices, and compatibility guidelines during development, refactoring, and codebase audits.

## Enforced Standards

This skill configures AI agents to adhere to current WordPress coding standards and WordPress.org plugin review guidelines. The full rules are in [SKILL.md](./SKILL.md), covering:

1. WordPress Coding Standards
2. PHP Compatibility (7.4+)
3. Unique Prefixing
4. Plugin Quality (Plugin Check)
5. Translation Text Domain
6. Avoid `strip_tags()`
7. Do Not Load Plugin Textdomain Manually
8. Date and Time Handling
9. Database Caching
10. Translation Placeholders
11. Version Updates and Changelog
12. Avoid Global PHP Limit Modifications
13. Declare Plugin Dependencies (`Requires Plugins`)
14. WordPress.org Reviewer Standards
15. PHPCS Ignore Annotations & WordPress VIP Rules
16. Readme Tag Limits

## Installation

You can install this skill either globally for all projects or locally within a specific project repository.

### Global Installation

Clone this repository directly into your agent's global skills configuration directory.

**Google Antigravity**

macOS/Linux:
```bash
git clone https://github.com/majiix/wp-best-practices.git ~/.gemini/config/skills/wp-best-practices
```
Windows (cmd/PowerShell):
```
git clone https://github.com/majiix/wp-best-practices.git "%USERPROFILE%\.gemini\config\skills\wp-best-practices"
```

**Claude Code**

macOS/Linux:
```bash
git clone https://github.com/majiix/wp-best-practices.git ~/.claude/skills/wp-best-practices
```
Windows (cmd/PowerShell):
```
git clone https://github.com/majiix/wp-best-practices.git "%USERPROFILE%\.claude\skills\wp-best-practices"
```

### Workspace-Specific Installation

To activate the skill for a single project, the directory depends on which agent you're using:

**Google Antigravity** (and other agents following the `.agents/skills` convention):

1. Create an `.agents/skills` directory at the root of your WordPress project repository if it does not exist.
2. Clone this repository into that directory:
```bash
git clone https://github.com/majiix/wp-best-practices.git .agents/skills/wp-best-practices
```

**Claude Code**:

Claude Code discovers project-level skills from `.claude/skills/` in the repository root instead:
```bash
git clone https://github.com/majiix/wp-best-practices.git .claude/skills/wp-best-practices
```

If you use multiple agents on the same project, you can clone into both directories, or clone once and symlink the second path to avoid duplication.

## Configuration

The core instructions are located in the [SKILL.md](https://github.com/majiix/wp-best-practices/blob/main/SKILL.md) file. The frontmatter defines the trigger and activation rules:

```
---
name: wp-best-practices
description: "Use when coding, refactoring, or auditing WordPress projects (plugins, themes, or apps). Enforces PHP 7.4+ compatibility, unique prefixing (4+ chars), translation rules, database caching, and specific date/time handling."
---
```

## Contributing

Contributions to refine or expand these best practices are welcome. Please open an issue or submit a pull request with your suggested improvements.

## License

This project is licensed under the MIT License.