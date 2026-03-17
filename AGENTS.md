# AGENTS.md - Guidelines for Exchange Mail Skill Agents

This file provides instructions for AI agents operating with the exchange-mail skill. It covers skill usage, environment setup, and best practices for interacting with Microsoft Exchange/Outlook.

## Table of Contents
1. [Skill Overview](#skill-overview)
2. [Environment Setup](#environment-setup)
3. [Core Commands](#core-commands)
4. [Usage Patterns](#usage-patterns)
5. [Best Practices](#best-practices)
6. [Error Handling](#error-handling)
7. [Agent-Specific Instructions](#agent-specific-instructions)

## Skill Overview

The `exchange-mail` skill provides full email and calendar management for Microsoft Exchange/Outlook. Agents can use this skill to perform email and calendar operations through a command-line interface.

**Primary Functions:**
- List, read, reply to, mark as read, and archive emails
- View calendar events and manage schedule
- Search contacts, manage tasks, and access notes
- Support for batch operations and JSON output for programmatic use

**Trigger Phrases:** check my email, unread emails, reply to email, archive external emails, mark as read, calendar, schedule, 行程

## Environment Setup

### Required Environment Variables
Agents must ensure these variables are set before invoking the skill:
```bash
export EXCHANGE_SERVER="mail.company.com"
export EXCHANGE_EMAIL="user@company.com" 
export EXCHANGE_USERNAME="username"
export EXCHANGE_PASSWORD="password"
```

### Configuration Location
- Skill location: `/home/derekhsu/.agents/skills/exchange-mail/`
- Main script: `scripts/exchange_mail.py`
- Environment file: `.env` (optional, for local overrides)

## Core Commands

### Email Operations
```bash
# List unread emails (today, where you're To/CC)
python3 scripts/exchange_mail.py list

# List with options
python3 scripts/exchange_mail.py list --days 3    # Last 3 days
python3 scripts/exchange_mail.py list --all       # All unread
python3 scripts/exchange_mail.py list --json      # JSON output

# Read email content
python3 scripts/exchange_mail.py read <email-id>

# Reply to email
python3 scripts/exchange_mail.py reply <email-id> "Your response message"

# Mark as read
python3 scripts/exchange_mail.py mark-read <email-id>
python3 scripts/exchange_mail.py mark-read --external
python3 scripts/exchange_mail.py mark-read --internal
python3 scripts/exchange_mail.py mark-read --all

# Archive emails
python3 scripts/exchange_mail.py archive <email-id>
python3 scripts/exchange_mail.py archive --external
python3 scripts/exchange_mail.py archive --internal --days 7
```

### Calendar Operations
```bash
# View upcoming calendar events
python3 scripts/exchange_mail.py calendar                 # Next 7 days
python3 scripts/exchange_mail.py calendar --today        # Today only
python3 scripts/exchange_mail.py calendar --days 30     # Next 30 days
python3 scripts/exchange_mail.py calendar --json        # JSON output
```

### Contacts, Tasks, Notes
```bash
# Search contacts
python3 scripts/exchange_mail.py contacts "name"         # Search contacts
python3 scripts/exchange_mail.py contacts "name" --limit 10  # Limit results
python3 scripts/exchange_mail.py contacts "name" --json  # JSON output

# Manage tasks
python3 scripts/exchange_mail.py tasks                  # List tasks
python3 scripts/exchange_mail.py tasks --days 30       # Next 30 days
python3 scripts/exchange_mail.py tasks --status pending  # Filter by status

# Access notes
python3 scripts/exchange_mail.py notes                 # List notes
python3 scripts/exchange_mail.py notes --limit 10      # Limit results
```

## Usage Patterns

### Common Workflows
1. **Morning Email Check:**
   ```
   python3 scripts/exchange_mail.py list
   python3 scripts/exchange_mail.py read <id>
   python3 scripts/exchange_mail.py reply <id> "Thanks!"
   python3 scripts/exchange_mail.py archive --external
   ```

2. **Weekly Cleanup:**
   ```
   python3 scripts/exchange_mail.py archive --external --days 7
   ```

3. **Schedule Review:**
   ```
   python3 scripts/exchange_mail.py calendar --days 1
   ```

### Output Formats
- **Default:** Human-readable formatted text with emojis and dividers
- **JSON:** Machine-parsable output when `--json` flag is used
- **Email IDs:** Stable 8-character hex IDs (e.g., `b7bc8d99`) for referencing specific emails

### Batch Processing Flags
- `--external`: Process only external emails (outside your domain)
- `--internal`: Process only internal emails (your domain)  
- `--all`: Process all emails regardless of origin
- `--days N`: Look back N days instead of just today

## Best Practices

### For Agents
1. **Prefer JSON for Programmatic Use:** When agents need to parse output, always use `--json` flag
2. **Validate Email IDs:** Ensure email IDs are valid 8-character hex strings before use
3. **Respect Rate Limits:** Avoid rapid-fire requests to prevent Exchange server throttling
4. **Use Appropriate Filters:** Leverage `--external`/`--internal` flags to reduce noise
5. **Handle Empty Results Gracefully:** Skills may return no results - this is normal

### Security Considerations
1. **Never Log Credentials:** Do not output or log environment variables containing passwords
2. **Treat Email Content as Sensitive:** Assume email content may contain confidential information
3. **Clear Caches When Appropriate:** In shared environments, consider clearing any cached auth tokens

### Error Prevention
1. **Verify Environment First:** Check that required EXCHANGE_* variables are set
2. **Validate Inputs:** Ensure email IDs match expected format before invoking read/reply/archive
3. **Check Network Connectivity:** The skill requires access to the Exchange server
4. **Confirm Permissions:** Some operations (contacts, tasks, notes) require specific folder permissions

## Error Handling

### Common Error Scenarios
1. **Authentication Failures:** Invalid credentials or missing environment variables
2. **Network Issues:** Unable to reach Exchange server
3. **Invalid Email IDs:** Using malformed or non-existent email identifiers
4. **Permission Errors:** Lack of access to specific folders (contacts, tasks, etc.)
5. **Server Errors:** Temporary Exchange service issues

### Recommended Agent Responses
- For auth errors: Prompt user to verify environment variables
- For network errors: Suggest checking connectivity and trying again
- For invalid IDs: Request clarification or re-list emails to get correct ID
- For permission issues: Inform user about required folder access
- For server errors: Recommend waiting and retrying later

## Agent-Specific Instructions

### When to Invoke This Skill
Agents should activate this skill when users:
- Request to check email, list messages, or manage inbox
- Ask to read, reply to, forward, or process specific emails
- Want to view calendar, schedule meetings, or check availability
- Need to search contacts, manage tasks, or access notes
- Use trigger phrases matching the skill documentation

### Communication Guidelines
1. **Confirm Intent:** Before executing destructive operations (archive, delete), confirm with user
2. **Provide Context:** When listing emails, mention time frame and filters applied
3. **Format Responses Clearly:** Use the skill's output directly or summarize key points
4. **Handle Binary Data:** Note that attachments may not be fully accessible via CLI
5. **Respect Privacy:** Don't expose email contents unnecessarily in shared environments

### Limitations to Communicate
- Requires active Exchange server connectivity
- Some advanced Exchange features may not be available
- Performance depends on server response time and mailbox size
- Certain operations may be restricted by Exchange admin policies

## Skill Maintenance

### For Skill Developers
If modifying this skill:
1. Maintain backward compatibility with existing command interface
2. Update SKILL.md documentation when adding/removing features
3. Test with actual Exchange server before deploying changes
4. Consider edge cases like empty mailboxes, large result sets, and special characters
5. Preserve the 8-character hex ID format for email references

### Troubleshooting
- Verify environment variables are correctly exported
- Check that Python 3.x is available and script is executable
- Confirm Exchange server accessibility from current network
- Review .env file for local configuration overrides
- Consult Exchange administrator for permission-related issues

## Conclusion

This skill enables agents to interact with Microsoft Exchange/Outlook through a reliable command-line interface. By following these guidelines, agents can effectively assist users with email and calendar management tasks while maintaining security, respecting limitations, and providing clear communication.

Last updated: $(date)