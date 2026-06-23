# cbMailServices Documentation Project Instructions

## Project Overview

This repository contains the comprehensive documentation for **cbMailServices** - a ColdBox module for sending email in a fluent and abstracted approach. The documentation is built using **GitBook** and follows a structured approach to documenting module features, configuration, and protocols.

## Architecture & Organization

### Core Structure

- **`essentials/`** - Core usage documentation (installation, configuration, sending mail)
- **`advanced/`** - Advanced features (async mail, custom protocols, mail events)
- **`readme/`** - Project meta information, release history, about the book

### Documentation Types

1. **Essentials Guides** - Getting started, configuration, and basic usage (`installation.md`, `configuration.md`, `sending-mail.md`)
2. **Advanced Guides** - Feature-focused with practical examples (`async-mail.md`, `building-protocols.md`, `mail-events.md`)

## File Conventions

### Frontmatter Structure
Every documentation file uses YAML frontmatter with specific patterns:

```yaml
---
description: Brief, descriptive summary for SEO/navigation
---
```

### Content Patterns

- **Emojis in headings** for visual hierarchy: `## 📧 Sending Mail`
- **Table-based API references** with consistent columns
- **Code examples** in `javascript`/`java` blocks for CFML syntax highlighting. Use `xml` or `html` for markup examples.
- **GitBook callouts** using `{% hint style="info|warning|danger|success" %}`
- **Markdown spacing** - All headers must be surrounded by blank lines (before and after). All code blocks must be surrounded by blank lines (before and after the fences).

### Navigation Integration

- **`SUMMARY.md`** defines the complete table of contents structure
- Cross-references use `{% content-ref url="relative/path.md" %}`
- External embeds with `{% embed url="..." %}`
- API docs hosted at `https://s3.amazonaws.com/apidocs.ortussolutions.com/coldbox-modules/cbmailservices/`

## Documentation Workflows

### Content Creation Patterns
1. **Start with frontmatter** - Always include description
2. **Use consistent heading hierarchy** - H1 (title), H2 (major sections), H3 (subsections)
3. **Include practical examples** - Every feature should have working code samples
4. **Cross-link related content** - Reference other docs for comprehensive coverage

### Reference Documentation
- **Configuration docs** include all settings with descriptions and examples
- **Protocol docs** include registration patterns, property requirements, and usage
- **API methods** follow pattern: Signature → Parameters → Examples

### Special Content Types
- **Configuration examples** - Use javascript blocks with proper formatting
- **API tables** - Consistent column structure across all reference materials
- **Protocol properties** - Document required vs optional properties clearly

## Language-Specific Patterns

### CFML Syntax Conventions
- Use `javascript` syntax highlighting for CFML code blocks
- Function calls: `functionName( arg1, arg2 )`
- Structure access: `struct.key` or `struct[ "key" ]`
- Component syntax: `component extends="..."` for CFCs
- **Semicolons** - Use semicolons to terminate statements (standard CFML convention)
- Method arguments use `name : value` notation for named parameters
- Strings can use single or double quotes

## GitBook Integration

### GitBook MCP Server

This project uses the GitBook Model Context Protocol (MCP) server for enhanced documentation capabilities.

**Reference:** [GitBook MCP Documentation](https://gitbook.com/docs/~gitbook/mcp)

Use the GitBook MCP to:
- Learn GitBook-specific syntax and features
- Understand code block formatting and options
- Access GitBook blocks and components documentation
- Follow GitBook best practices for content creation

### Styling Elements
- **Hint blocks** for callouts: `{% hint style="type" %}content{% endhint %}`
  - Available styles: `info`, `warning`, `danger`, `success`
  - Reference: [GitBook Hint Blocks](https://gitbook.com/docs/creating-content/blocks/hint)
- **Content references** for internal navigation
- **Embed blocks** for external resources
- **Table components** with GitBook-specific formatting
- **Code blocks** with syntax highlighting and optional features

### File Organization
- **README.md files** serve as section introductions
- **Reference materials** organized by functional categories
- **Progressive disclosure** - overview → details → examples → advanced patterns

## Contributing Guidelines

### Content Standards
- **Comprehensive examples** - Every feature needs working code samples
- **Cross-platform considerations** - Note OS-specific behaviors where relevant
- **Error handling patterns** - Include exception handling in examples
- **Protocol differences** - Document behavior differences across mail protocols

### Documentation Maintenance
- **Version compatibility** - Note cbMailServices version requirements
- **External link validation** - Ensure embedded content remains accessible
- **Code example testing** - Verify all code samples work with current cbMailServices
- **Cross-reference accuracy** - Maintain valid internal links

This documentation serves as both user guide and developer reference, emphasizing practical usage patterns while maintaining comprehensive API coverage.

## AI Skills

This repository ships AI skill packs that teach coding agents specialized knowledge. Skills are stored in `.agents/skills/` (the canonical location).

**BLOCKING REQUIREMENT:** When a skill applies to the user's request, load the relevant `SKILL.md` file **immediately** as your first action — before generating any response or writing code. Use `read_file` to load it.

### Available Skills

| Skill | Path | When to use |
| --- | --- | --- |
| `boxlang-core-dev-async-tasks` | `.agents/skills/boxlang-core-dev-async-tasks/SKILL.md` | BoxLang asynchronous programming: BoxFuture (CompletableFuture extension), AsyncService, executor types (VIRTUAL/FIXED/CACHED/SCHEDULED), BaseScheduler, ScheduledTask fluent API, scheduling with cron constraints, task lifecycle callbacks, and registering schedulers via ModuleConfig.bx |
| `boxlang-core-dev-bif-development` | `.agents/skills/boxlang-core-dev-bif-development/SKILL.md` | Creating custom BoxLang built-in functions (BIFs): @BoxBIF annotation, invoke() method, argument handling, accessing BoxRuntime and services, BoxLang vs Java BIF implementations, member functions, and registering BIFs via modules |
| `boxlang-core-dev-component-development` | `.agents/skills/boxlang-core-dev-component-development/SKILL.md` | Creating custom BoxLang components (tags): component file structure, attribute declarations, body/output handling, registering component paths in modules, the difference between components and classes, and testing custom components |
| `boxlang-core-dev-interceptors` | `.agents/skills/boxlang-core-dev-interceptors/SKILL.md` | Creating BoxLang interceptors: Observer/Intercepting Filter patterns, interceptor pools, BoxLang class vs Java interceptors, lambda interceptors, registration via BIFs/InterceptorService/ModuleConfig, interception points, and announcing custom events |
| `boxlang-core-dev-logging` | `.agents/skills/boxlang-core-dev-logging/SKILL.md` | Working with BoxLang logging: obtaining loggers via LoggingService, using BoxLangLogger (trace/debug/info/warn/error), pre-configured common loggers, creating named loggers, parameterized messages, accessing loggers from context and interceptors, and logging configuration in boxlang.json |
| `boxlang-core-dev-module-development` | `.agents/skills/boxlang-core-dev-module-development/SKILL.md` | Creating a BoxLang module: ModuleConfig.bx structure, lifecycle methods (configure/onLoad/onUnload), module metadata, registering interceptors and BIFs, Gradle build setup, publishing to ForgeBox, or using the official module template |
| `boxlang-core-dev-runtime-architecture` | `.agents/skills/boxlang-core-dev-runtime-architecture/SKILL.md` | Understanding BoxLang internals: BoxRuntime services, IBoxContext hierarchy, scope chain resolution, DynamicObject, type system (IBoxType), parsing pipeline (source to bytecode), class loader isolation, virtual threads, or using --bx-printast for AST debugging |
| `gitbook-docs-expert` | `.agents/skills/gitbook-docs-expert/SKILL.md` | GitBook documentation syntax: frontmatter, hint blocks, content-ref, embed blocks, tabs, code blocks, tables, SUMMARY.md navigation, and GitBook-specific formatting |
| `ortus-java-coding-standards` | `.agents/skills/ortus-java-coding-standards/SKILL.md` | Writing, reviewing, or formatting any Ortus Solutions code (BoxLang, CFML, or Java) to ensure it follows the official Ortus coding standards: indentation, spacing, brace placement, naming, alignment, comments, and structural conventions |

### Adding New Skills

To add a new skill to this repository:

1. Create the skill folder and `SKILL.md` in `.agents/skills/<skill-name>/`.
2. Register the skill in the `Available Skills` table above.
3. Add the skill to `skills-lock.json`.

## MCP Integrations

- This book is published at https://cbmailservices.ortusbooks.com
- It has its own MCP server: https://cbmailservices.ortusbooks.com/~gitbook/mcp

Here are other Ortus MCP servers to integrate with:

- GitBook Docs: https://gitbook.com/docs/~gitbook/mcp
- BoxLang: https://boxlang.ortusbooks.com/~gitbook/mcp
- BoxLang AI: https://ai.ortusbooks.com/~gitbook/mcp
- CacheBox: https://cachebox.ortusbooks.com/~gitbook/mcp
- ColdBox: https://coldbox.ortusbooks.com/~gitbook/mcp
- CommandBox: https://commandbox.ortusbooks.com/~gitbook/mcp
- LogBox: https://logbox.ortusbooks.com/~gitbook/mcp
- TestBox: https://testbox.ortusbooks.com/~gitbook/mcp
- WireBox: https://wirebox.ortusbooks.com/~gitbook/mcp