# Querying Granola

A Claude Code skill for querying your local [Granola](https://granola.ai) meeting cache. Search meeting notes, track engagement with contacts, and prepare for meetings using your meeting history.

> **Note**: This is an unofficial community tool. Granola does not provide a public API, so this skill reads directly from Granola's local cache file. The cache format is undocumented and could change with any Granola update. This approach is used by several community projects ([MCP servers](https://github.com/proofgeist/granola-ai-mcp-server), [Raycast extensions](https://www.raycast.com/Rob/granola)) but is not officially supported by Granola.

## What is this?

[Granola](https://granola.ai) is an AI notepad for meetings that automatically captures transcripts and generates summaries. This skill lets Claude Code query your local Granola cache to:

- **Search meetings** by title, content, or attendee
- **Get full context** for a specific meeting (notes + transcript)
- **Track engagement** with people and organizations
- **Prepare for meetings** by reviewing past interactions

## Requirements

- [Granola](https://granola.ai) installed with meeting history
- Python 3
- [Claude Code](https://claude.ai/claude-code) CLI

## Installation

1. Clone this repository into your Claude Code skills directory:

```bash
# Navigate to your project or home directory
git clone https://github.com/yourusername/querying-granola.git

# Or add as a skill to an existing project
cp -r querying-granola/skills/querying-granola ~/.claude/skills/
```

2. The skill will be automatically discovered by Claude Code.

## Usage

Once installed, just ask Claude Code natural language questions about your meetings:

- "What did we discuss with Acme Corp last month?"
- "Find all meetings about the quarterly review"
- "Who have I been meeting with the most?"
- "Show me my meeting history with John Smith"
- "Help me prepare for my meeting with acme.com"

## Available Commands

The skill exposes these commands through the Python script:

| Command | Purpose |
|---------|---------|
| `search <query>` | Search meetings by keyword |
| `client <name>` | Get meetings matching name in title/notes/attendees |
| `context <title>` | Get full notes + transcript for a specific meeting |
| `profile <domain>` | Company profile with contacts, topics, history |
| `domains` | Meeting counts by email domain |
| `people` | Meeting counts by person |
| `active [N]` | Most active contacts in last N days (default 30) |
| `stale [N]` | Contacts with no meetings in N+ days (default 60) |
| `timeline <query>` | Meeting frequency over time (visual bar chart) |
| `recent [N]` | List N most recent meetings (default 20) |

## Data Sources

The skill reads from Granola's local cache at:
```
~/Library/Application Support/Granola/cache-v3.json
```

**Important**: This cache file is an internal implementation detail of Granola, not a stable API. The structure may change without notice in future Granola updates, which could break this skill. If that happens, the skill will need to be updated to match the new format.

It extracts data from multiple cache sections:
- **documents**: Meeting title, date, user-typed notes, attendees
- **documentPanels**: AI-generated summaries
- **meetingsMetadata**: Enriched attendee info with company names
- **transcripts**: Raw meeting transcripts (recent meetings only)

## Privacy

This skill only reads your local Granola cache. No data is sent anywhere except to Claude during your conversation. The cache file never leaves your machine.

## License

MIT
