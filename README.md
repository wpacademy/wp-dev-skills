<img width="1200" height="720" alt="banner" src="https://github.com/user-attachments/assets/7261f143-92b6-416c-ace9-e3d3babc94d0" />

# WordPress AI Agent Skills

A collection of AI agent skills (custom rules/instructions) for professional WordPress development. These skills ensure your AI coding agent — whether it's Claude, Cursor, Windsurf, Antigravity, GitHub Copilot, or any other — follows official WordPress coding standards, security best practices, accessibility requirements (WCAG 2.1 AA), and WordPress.org directory guidelines when building plugins and themes.

## 🎯 What Are AI Agent Skills?

AI Agent Skills are reusable instruction sets (written in plain Markdown) that guide AI coding assistants to follow specific standards and patterns. When you add these skills to your project, your AI agent automatically follows WordPress best practices — producing code that's secure, accessible, modular, and ready for WordPress.org submission.

They work across all major AI coding tools:

| Tool | Config Location |
|---|---|
| **Claude Code** | `.claude/skills/` |
| **Cursor** | `.cursor/rules/` |
| **Windsurf** | `.windsurf/rules/` |
| **Antigravity** | `.antigravity/rules/` |
| **GitHub Copilot** | `.github/copilot-instructions.md` |

## 📦 Included Skills

### 1. `wp-plugin-dev` — WordPress Plugin Development

Guides your AI agent to build production-ready WordPress plugins with proper architecture and security.

**Triggers when you say things like:**
- "Build me a plugin for X"
- "Create a WordPress plugin"
- "I need a WooCommerce extension"
- "Add a REST API endpoint"

**What it enforces:**

- **Clean bootstrap** — Main plugin file contains only constants, hooks, and autoloading. No feature logic.
- **Modular architecture** — Every feature gets its own class file (`includes/`, `admin/`, `public/`, `api/`)
- **Security** — Mandatory `sanitize_*()` on all input, `esc_*()` on all output, `$wpdb->prepare()` for all SQL, nonces on all forms, capability checks on all privileged actions
- **Caching** — Transients API and `wp_cache_*` for expensive operations
- **WordPress.org compliance** — GPL licensing, `readme.txt`, `uninstall.php`, proper prefixing, translatable strings

**Supported plugin types:**

| Type | Boilerplate Included |
|---|---|
| Standard (settings, CPTs, shortcodes) | ✅ |
| WooCommerce extensions | ✅ |
| Gutenberg block plugins | ✅ |
| REST API / headless plugins | ✅ |
| AJAX handlers | ✅ |
| Custom database tables | ✅ |

<details>
<summary><strong>File structure</strong></summary>

```
wp-plugin-dev/
├── SKILL.md                          # Main instructions (177 lines)
└── references/
    ├── architecture.md               # Boilerplate for all plugin types (742 lines)
    ├── security.md                   # Sanitization, escaping, $wpdb, nonces, caching (216 lines)
    └── wp-org-guidelines.md          # Condensed 18 official guidelines (31 lines)
```

</details>

---

### 2. `wp-theme-dev` — WordPress Theme Development

Guides your AI agent to build accessible, standards-compliant WordPress themes.

**Triggers when you say things like:**
- "Build me a theme for X"
- "Create a block theme"
- "I need a classic WordPress theme"
- "Make a child theme"

**What it enforces:**

- **Block themes as default** — Full Site Editing with `theme.json`, HTML templates, block patterns, style variations
- **Classic theme support** — Template hierarchy, The Loop, Customizer API when requested
- **Accessibility (WCAG 2.1 AA)** — Skip links, keyboard navigation, visible focus indicators, ARIA landmarks, 4.5:1 color contrast, underlined content links, proper heading hierarchy, screen-reader-text, reduced motion support
- **Modern CSS** — Custom properties, Grid/Flexbox, `clamp()` fluid typography, logical properties for RTL
- **WordPress.org compliance** — All 14 review categories, no plugin territory, proper licensing, `readme.txt` with resource credits

**Supported theme types:**

| Type | Boilerplate Included |
|---|---|
| Block themes (FSE) | ✅ |
| Classic PHP themes | ✅ |
| Child themes | ✅ |
| Hybrid themes | ✅ |

<details>
<summary><strong>File structure</strong></summary>

```
wp-theme-dev/
├── SKILL.md                              # Main instructions (193 lines)
└── references/
    ├── block-theme-architecture.md       # theme.json, templates, patterns, style variations (567 lines)
    ├── classic-theme-architecture.md     # Template hierarchy, The Loop, header/footer (497 lines)
    ├── accessibility.md                  # WCAG 2.1 AA + WordPress accessibility-ready (352 lines)
    └── review-requirements.md            # All 14 WordPress.org review categories (175 lines)
```

</details>

## 🚀 Installation

### For Claude Code (`.claude/` directory)

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/wordpress-ai-agent-skills.git

# Copy skills to your project
cp -r wordpress-ai-agent-skills/wp-plugin-dev your-project/.claude/skills/
cp -r wordpress-ai-agent-skills/wp-theme-dev your-project/.claude/skills/
```

Your project structure should look like:

```
your-project/
└── .claude/
    └── skills/
        ├── wp-plugin-dev/
        │   ├── SKILL.md
        │   └── references/
        │       ├── architecture.md
        │       ├── security.md
        │       └── wp-org-guidelines.md
        └── wp-theme-dev/
            ├── SKILL.md
            └── references/
                ├── block-theme-architecture.md
                ├── classic-theme-architecture.md
                ├── accessibility.md
                └── review-requirements.md
```

### For Claude.ai (User Skills)

1. Download the skill folder(s) from this repo
2. Upload to Claude.ai via **Settings → Skills** (when available) or reference them in your project

---

## 🔄 Setup by Platform

These skills are **plain Markdown files** that work with any AI coding tool supporting custom rules or instructions. Choose your platform below:

### Cursor IDE

Cursor uses `.cursor/rules/` for project-specific rules. You can convert these skills into Cursor rules:

```bash
# Option 1: Single rule file (combine SKILL.md + references)
mkdir -p your-project/.cursor/rules

# For plugin development
cat wp-plugin-dev/SKILL.md wp-plugin-dev/references/*.md > your-project/.cursor/rules/wp-plugin-dev.mdc

# For theme development
cat wp-theme-dev/SKILL.md wp-theme-dev/references/*.md > your-project/.cursor/rules/wp-theme-dev.mdc
```

Add a frontmatter header to the `.mdc` file for auto-activation:

```yaml
---
description: WordPress plugin development standards and best practices
globs: "**/*.php"
alwaysApply: false
---
```

> **Note:** Cursor has a 6,000 character limit per rule file. You may need to split into multiple rule files (e.g., `wp-plugin-security.mdc`, `wp-plugin-architecture.mdc`) or paste the SKILL.md as the rule and keep references as separate files to `@reference` in prompts.

### Windsurf (Cascade)

Windsurf uses `.windsurf/rules/` for workspace rules:

```bash
mkdir -p your-project/.windsurf/rules

# Combine into a single rule file
cat wp-plugin-dev/SKILL.md wp-plugin-dev/references/*.md > your-project/.windsurf/rules/wp-plugin-dev.md
cat wp-theme-dev/SKILL.md wp-theme-dev/references/*.md > your-project/.windsurf/rules/wp-theme-dev.md
```

Or paste the contents via **Windsurf Settings → Set Workspace AI Rules → Edit Rules**.

> **Note:** Windsurf has a 12,000 combined character limit for all rules. You may need to use a condensed version of the SKILL.md only.

### Google Antigravity

Antigravity supports custom rules similar to Cursor/Windsurf. Place rule files in your project root or configure via the Agent Manager settings:

```bash
# Copy the combined rules
cat wp-plugin-dev/SKILL.md wp-plugin-dev/references/*.md > your-project/.antigravity/rules/wp-plugin-dev.md
```

### GitHub Copilot

GitHub Copilot supports custom instructions via `.github/copilot-instructions.md`:

```bash
# Combine into a single instructions file
cat wp-plugin-dev/SKILL.md wp-plugin-dev/references/*.md > your-project/.github/copilot-instructions.md
```

### Any Other AI Tool

The skills are just Markdown files. For any AI coding assistant that accepts custom instructions or system prompts:

1. **Paste the SKILL.md content** as the primary instruction/system prompt
2. **Attach reference files** as additional context when working on specific tasks
3. Or **combine everything** into a single document and paste into your tool's rules/settings

### Platform Comparison

| Feature | Claude | Cursor | Windsurf | Antigravity | Copilot |
|---|---|---|---|---|---|
| Config location | `.claude/skills/` | `.cursor/rules/` | `.windsurf/rules/` | `.antigravity/rules/` | `.github/copilot-instructions.md` |
| File format | Markdown (YAML frontmatter) | `.mdc` (Markdown) | Markdown | Markdown | Markdown |
| Multi-file support | ✅ (skill + references) | ✅ (multiple rule files) | ✅ (multiple rule files) | ✅ | ❌ (single file) |
| Auto-trigger by context | ✅ (description field) | ✅ (globs + description) | ✅ (activation modes) | ✅ | ❌ (always active) |
| Character limit | ~Generous | 6,000/rule | 12,000 total | Varies | No hard limit |

> **💡 Tip:** The modular structure (SKILL.md + separate reference files) is designed so you can load only what you need. For tools with tight character limits, use just the SKILL.md and reference specific files when needed.

## 💡 Usage Examples

### Plugin Development

```
Build me a WordPress plugin called "Smart Reviews" that adds a custom
post type for reviews with star ratings, a REST API endpoint to fetch
reviews, and a Gutenberg block to display them.
```

Claude will automatically:
- Scaffold a clean, modular plugin structure
- Keep the main file as a bootstrap loader only
- Create separate class files for the CPT, REST API, and block
- Sanitize all input, escape all output, use `$wpdb->prepare()`
- Generate `readme.txt` and `uninstall.php`

> Other AI agents (Cursor, Windsurf, etc.) will follow the same patterns when these rules are loaded.

### Theme Development

```
Create a block theme called "Starter Blue" with a hero section,
blog archive, and dark mode style variation. Make it accessibility-ready.
```

Your AI agent will automatically:
- Generate `theme.json` with accessible color palette (4.5:1+ contrast)
- Create HTML templates with proper semantic structure
- Add skip links, ARIA landmarks, underlined content links
- Include keyboard-navigable mobile menu with `aria-expanded`
- Build block patterns with translatable strings
- Generate `readme.txt` with resource credits

## 📚 Sources & References

These skills are built from official WordPress documentation:

- [WordPress Plugin Handbook](https://developer.wordpress.org/plugins/)
- [WordPress Plugin Guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/)
- [WordPress Theme Handbook](https://developer.wordpress.org/themes/)
- [Theme Review Requirements](https://make.wordpress.org/themes/handbook/review/required/)
- [Accessibility-Ready Requirements](https://make.wordpress.org/themes/handbook/review/accessibility/required/)
- [WordPress Security APIs](https://developer.wordpress.org/apis/security/)
- [WCAG 2.1 Level AA](https://www.w3.org/WAI/WCAG21/quickref/?levels=aaa)

## 🤝 Contributing

Contributions are welcome! If you'd like to improve these skills:

1. Fork the repository
2. Create a feature branch (`git checkout -b improve-security-reference`)
3. Make your changes
4. Test with your preferred AI agent to verify the skill produces correct output
5. Submit a pull request

**Areas that could use contributions:**
- Additional boilerplate patterns (e.g., WP-CLI commands, multisite support)
- Expanded Gutenberg block development patterns
- Performance optimization reference
- Testing/CI pipeline patterns

## 📄 License

This project is licensed under the [GPLv2 or later](https://www.gnu.org/licenses/gpl-2.0.html), consistent with WordPress itself.

## 👤 Author

**Mian** — Full-stack WordPress developer with 15+ years of experience.

- YouTube: [WP Academy](https://youtube.com/@wpacademy) (125K+ subscribers)
- Website: [msrbuilds.com](https://msrbuilds.com)
