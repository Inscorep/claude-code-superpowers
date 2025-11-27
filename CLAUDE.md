# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a fork of [obra/superpowers](https://github.com/obra/superpowers), customised for Inscorep workflows. We maintain this fork to adjust skills to our specific needs while pulling upstream improvements as needed.

Superpowers is a Claude Code plugin providing composable "skills" that enforce software development workflows. Skills are markdown files with YAML frontmatter that get injected into Claude's context to guide behaviour during development tasks.

## Architecture

### Plugin Structure

```
.claude-plugin/marketplace.json  # Plugin metadata and versioning
hooks/
  hooks.json                     # Hook configuration (SessionStart)
  session-start.sh               # Injects using-superpowers skill on session start
lib/
  skills-core.js                 # Skill discovery, resolution, and frontmatter parsing
skills/                          # Skill library (each skill in own directory)
  <skill-name>/
    SKILL.md                     # Main skill content (required)
    *.md|*.ts|*.sh               # Supporting files (optional)
agents/
  code-reviewer.md               # Agent definition for code review subagent
commands/
  brainstorm.md                  # Slash command wrappers for skills
  execute-plan.md
  write-plan.md
```

### Skill System

Skills are loaded via the `Skill` tool. Personal skills in `~/.claude/skills/` override plugin skills with the same name. Use `superpowers:skill-name` prefix to force loading the plugin version.

**Skill YAML frontmatter** (only fields supported):
- `name`: Letters, numbers, hyphens only
- `description`: Third-person, starts with "Use when...", max 1024 chars total

### Hook System

The `session-start.sh` hook runs on SessionStart events (startup, resume, clear, compact) and injects the `using-superpowers` skill content into the conversation context.

### Key Files

- `lib/skills-core.js` - Core functions: `extractFrontmatter()`, `findSkillsInDir()`, `resolveSkillPath()`, `stripFrontmatter()`
- `skills/using-superpowers/SKILL.md` - Entry point skill, loaded every session
- `skills/writing-skills/SKILL.md` - Guide for creating new skills (TDD-based approach)

## Development Workflow

When modifying or creating skills:

1. Skills follow TDD: write failing test scenarios first, then the skill
2. Test skills with subagents before deployment
3. Keep skills under 500 words; frequently-loaded skills under 200 words
4. Description must include triggering conditions ("Use when...")

## Testing

No automated test suite. Skills are tested via subagent pressure scenarios as documented in `skills/testing-skills-with-subagents/SKILL.md`.

## Version

Current version: 3.5.1 (in `.claude-plugin/marketplace.json`)
