# Documentation Style Guide

This is a personal knowledge base, not a public-facing guide. Follow these conventions when writing or editing documentation.

## Core Principles

- **Concise over verbose**: Focus on essentials, eliminate redundancy
- **Technical audience**: Assume readers have foundational knowledge
- **Principles and steps**: Prioritize concepts and procedures over tutorials

## Writing Style

### Avoid

- Tutorial language: "This guide will help you...", "In this tutorial..."
- Second person: "your username", "your domain", "you can..."
- Overly detailed prerequisites (e.g., "Have a GitHub account")
- Multiple ways to do the same thing (pick one)
- Simple example files (e.g., basic HTML templates)
- Explanations of obvious concepts

### Prefer

- Direct statements: "Replace `<username>` with the actual username"
- Tables for structured information
- Code blocks with minimal comments
- Bullet points for quick reference
- Reference links to official documentation

## Structure

### Domain Binding Documents

1. **Principle**: Brief explanation of how it works
2. **DNS Records**: Table format with type, name, value
3. **Steps**: Numbered list, essential actions only
4. **Verification**: Commands to check status
5. **Common Issues**: Table with problem, cause, solution
6. **References**: Links to official docs

### Quick Start Documents

1. **Key Rules**: Table or bullet list
2. **Steps**: Minimal numbered list
3. **Priority/Order**: If applicable
4. **Status Check**: How to verify

## Formatting

### Tables for Comparisons

```markdown
| Type | Name | Value |
|------|------|-------|
| A | @ | 185.199.108.153 |
```

### DNS Records

```
Type: A, Name: @, Value: 185.199.108.153
```

### Commands

```bash
dig example.com +short
```

## Tone

- Neutral and factual
- No greetings or closings
- No motivational language
- Technical precision over friendliness

## Examples

### Before (Public Guide Style)

> This guide will help you bind your custom domain to GitHub Pages. First, make sure you have a GitHub account and own a domain name. You can buy domains from providers like GoDaddy or Namecheap.

### After (Knowledge Base Style)

> ## Principle
> 
> DNS records point the custom domain to GitHub Pages servers. GitHub automatically provisions SSL certificates.
> 
> ## DNS Configuration
> 
> | Domain Type | Record Type | Name | Value |
> |-------------|-------------|------|-------|
> | Apex | A | @ | 185.199.108.153 |
