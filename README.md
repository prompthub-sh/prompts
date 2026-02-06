# prompts

Official prompt collection for [prompthub.sh](https://prompthub.sh).

## Installation

```bash
npx prompthub add prompthub-sh/prompts
```

Or install a specific prompt:

```bash
npx prompthub add prompthub-sh/prompts/react-expert
```

## Available Prompts

### Development

- **[react-expert](./prompts/react-expert/)** - Expert React and Next.js development guidance
- **[typescript-strict](./prompts/typescript-strict/)** - Strict TypeScript best practices
- **[python-modern](./prompts/python-modern/)** - Modern Python development patterns
- **[rust-systems](./prompts/rust-systems/)** - Systems programming with Rust

### Code Quality

- **[code-reviewer](./prompts/code-reviewer/)** - Thorough code review guidelines
- **[testing-expert](./prompts/testing-expert/)** - Testing strategies and patterns
- **[security-auditor](./prompts/security-auditor/)** - Security-focused code analysis

### Writing & Documentation

- **[technical-writer](./prompts/technical-writer/)** - Clear technical documentation
- **[api-documenter](./prompts/api-documenter/)** - API documentation best practices

## Prompt Structure

Each prompt contains:

```
prompts/
└── react-expert/
    └── PROMPT.md      # The prompt instructions
```

## Creating Your Own Prompts

1. Fork this repo or create your own
2. Create a folder with your prompt name
3. Add a `PROMPT.md` file with YAML frontmatter:

```markdown
---
name: my-prompt
version: 1.0.0
description: What this prompt does
author: your-username
tags: [tag1, tag2]
compatible_with: [claude, cursor, copilot, windsurf]
---

Your prompt content here...
```

4. Publish: `npx prompthub publish`

## License

MIT
