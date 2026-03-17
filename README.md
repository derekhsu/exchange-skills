# Exchange Mail Skill Project

This repository contains the `exchange-mail` skill for agentic AI systems, enabling full email and calendar management for Microsoft Exchange/Outlook.

## Overview
This skill is inspired by and references the work in https://github.com/DmitryBMsk/claude-code-codex-exchange-skill


The `exchange-mail` skill allows AI agents to interact with Microsoft Exchange/Outlook through a command-line interface, providing capabilities such as:

- Listing, reading, replying to, marking as read, and archiving emails
- Viewing calendar events and managing schedules
- Searching contacts, managing tasks, and accessing notes
- Supporting batch operations and JSON output for programmatic use

## Skill Location

The actual skill implementation is located at:
- `/home/derekhsu/.agents/skills/exchange-mail/`
- Symlinked in this repository as: `skills/exchange-skills`

## Key Files

- `AGENTS.md` - Comprehensive guidelines for AI agents operating with this skill
- `.gitignore` - Excludes Python cache, environment files, and other development artifacts
- `skills/exchange-skills/` - Symlink to the actual skill implementation

## Usage

Agents should refer to `AGENTS.md` for detailed instructions on how to operate with this skill, including:

- Required environment variables (`EXCHANGE_SERVER`, `EXCHANGE_EMAIL`, etc.)
- Core command usage (list, read, reply, archive, calendar, contacts, tasks, notes)
- Best practices for secure and effective operation
- Error handling guidance
- When and how to invoke the skill based on user requests

## Environment Setup

Before using this skill, ensure the following environment variables are set:

```bash
export EXCHANGE_SERVER="mail.company.com"
export EXCHANGE_EMAIL="user@company.com"
export EXCHANGE_USERNAME="username"
export EXCHANGE_PASSWORD="password"
```

These can be set in your shell configuration or in a `.env` file (note: `.env` is ignored by git for security).

## For Developers

If modifying this skill:
1. The main implementation is in `scripts/exchange_mail.py` within the skill directory
2. Refer to `SKILL.md` in the skill directory for detailed documentation
3. Maintain backward compatibility with the existing command-line interface
4. Test with an actual Exchange server before deploying changes

## Trigger Phrases

This skill activates when users make requests like:
- "check my email"
- "unread emails"
- "reply to email"
- "archive external emails"
- "mark as read"
- "calendar"
- "schedule"
- "行程" (Chinese for schedule)

## Publishing to skills.sh

This skill is automatically indexed by [skills.sh](https://skills.sh) when users install it via:

```bash
npx skills add derekhsu/exchange-skills
```

No manual submission is required! Skills.sh automatically:
- Scans public GitHub repositories for SKILL.md files
- Indexes skills when they're installed via `npx skills add`
- Tracks usage statistics and popularity

To verify your skill appears on skills.sh, you can:
1. Visit [skills.sh](https://skills.sh)
2. Search for "exchange-mail" or "exchange-skills"
3. Check the skill's installation count and popularity

## Installation for Users

Users can install this skill in any compatible AI agent:

```bash
npx skills add derekhsu/exchange-skills
```

Or for a specific skill in a multi-skill repository:

```bash
npx skills add derekhsu/exchange-skills --skill exchange-mail
```

Compatible with 18+ AI agents including Claude Code, GitHub Copilot, Cursor, Cline, and many others.

---
*Last updated: $(date)*