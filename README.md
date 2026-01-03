# Querying Granola

**Query your meeting history with natural language**

A [Claude Skill](https://docs.claude.com/en/docs/claude-code/skills) that lets Claude search and analyze your local [Granola](https://granola.ai) meeting data. Find past discussions, prepare for upcoming meetings, and track engagement with contacts - all through conversation.

Claude autonomously decides when to use this skill based on your requests about meetings, attendees, or discussion topics.

> **Note**: This is an unofficial community tool. Granola does not provide a public API, so this skill reads directly from Granola's local cache file. The cache format is undocumented and could change with any Granola update. This approach is used by several community projects ([MCP servers](https://github.com/proofgeist/granola-ai-mcp-server), [Raycast extensions](https://www.raycast.com/Rob/granola)) but is not officially supported by Granola.

## What is Granola?

[Granola](https://granola.ai) is an AI notepad for meetings that automatically captures transcripts and generates summaries. It runs locally on your machine and stores meeting data in a local cache. This skill lets Claude Code tap into that cache to help you:

- **Search meetings** by title, content, or attendee
- **Get full context** for a specific meeting (notes + transcript)
- **Track engagement** with people and organizations
- **Prepare for meetings** by reviewing past interactions

## Features

- **Natural Language Queries** - Ask about meetings in plain English, Claude handles the rest
- **Full-Text Search** - Search across meeting titles, notes, and attendee names
- **Meeting Context** - Get complete notes and transcripts for any meeting
- **Engagement Tracking** - See who you've been meeting with and how often
- **Company Profiles** - Get a complete picture of your interactions with any organization
- **Cross-Platform** - Works on macOS, Windows, and Linux

## Requirements

- [Granola](https://granola.ai) installed with meeting history
- Python 3
- [Claude Code](https://docs.claude.com/en/docs/claude-code) CLI

## Installation

### Global Installation (Available Everywhere)

```bash
# Clone to your global skills directory
git clone https://github.com/asterlabs-ai/querying-granola.git /tmp/querying-granola-temp

# Copy the skill folder
mkdir -p ~/.claude/skills
cp -r /tmp/querying-granola-temp/skills/querying-granola ~/.claude/skills/

# Clean up
rm -rf /tmp/querying-granola-temp
```

### Project-Specific Installation

```bash
# Clone to your project
git clone https://github.com/asterlabs-ai/querying-granola.git /tmp/querying-granola-temp

# Copy the skill folder to your project
mkdir -p .claude/skills
cp -r /tmp/querying-granola-temp/skills/querying-granola .claude/skills/

# Clean up
rm -rf /tmp/querying-granola-temp
```

### Verify Installation

Start Claude Code and ask something like "Who have I been meeting with lately?" to confirm the skill is working.

## Usage Examples

Once installed, just ask Claude about your meetings in natural language.

### Finding Past Discussions

```
"What did we discuss with Acme Corp last month?"
"Find all meetings about the quarterly review"
"When did we last talk about the API integration?"
```

### Meeting Preparation

```
"Help me prepare for my meeting with acme.com"
"What's our history with John Smith?"
"Show me the context from our last engineering sync"
```

### Engagement Tracking

```
"Who have I been meeting with the most?"
"Which companies haven't I talked to in a while?"
"Show me my meeting activity over the past 6 months"
```

### Contact Lookup

```
"Show me everyone I've met with from Tesla"
"What's my meeting history with sarah@example.com?"
"List my most frequent meeting contacts"
```

## How It Works

1. You ask Claude about your meetings in natural language
2. Claude recognizes this as a meeting-related query and loads the skill
3. The skill executes a Python script that reads your local Granola cache
4. Results are returned to Claude, who summarizes and presents them
5. Your data never leaves your machine (except to Claude during the conversation)

## Available Commands

The skill exposes these commands through the Python script:

| Command | Purpose |
|---------|---------|
| `search <query>` | Search meetings by keyword |
| `client <name>` | Get meetings matching name in title/notes/attendees |
| `context <title>` | Get full notes + transcript for a specific meeting |
| `profile <domain>` | Company profile with contacts and meeting history |
| `domains` | Meeting counts by email domain |
| `people` | Meeting counts by person |
| `active [N]` | Most active contacts in last N days (default 30) |
| `stale [N]` | Contacts with no meetings in N+ days (default 60) |
| `timeline <query>` | Meeting frequency over time (visual bar chart) |
| `recent [N]` | List N most recent meetings (default 20) |

## Project Structure

```
querying-granola/
├── skills/
│   └── querying-granola/        # The skill (Claude discovers this)
│       ├── SKILL.md             # Skill instructions for Claude
│       └── scripts/
│           └── granola.py       # Query script
├── README.md                    # This file
└── LICENSE                      # MIT License
```

## Data Sources

The skill reads from Granola's local cache:

| Platform | Location |
|----------|----------|
| macOS | `~/Library/Application Support/Granola/cache-v3.json` |
| Windows | `%APPDATA%/Granola/cache-v3.json` |
| Linux | `~/.config/Granola/cache-v3.json` |

**Important**: This cache file is an internal implementation detail of Granola, not a stable API. The structure may change without notice in future Granola updates, which could break this skill.

The cache contains:
- **documents** - Meeting title, date, user-typed notes, attendees
- **documentPanels** - AI-generated summaries
- **meetingsMetadata** - Enriched attendee info with company names
- **transcripts** - Raw meeting transcripts (recent meetings only, ~8)

## Troubleshooting

**Skill not loading?**
Check that the skill folder is in `~/.claude/skills/querying-granola/` (global) or `.claude/skills/querying-granola/` (project).

**"Cache not found" error?**
Make sure Granola is installed and you've recorded at least one meeting. The cache file is created after your first meeting.

**No results for a query?**
The skill only searches meetings with notes. Meetings without notes or AI summaries are excluded from most queries.

**Transcripts missing?**
Granola only caches transcripts for recent meetings (~8). Older meetings will have notes but no transcript.

## Privacy

This skill only reads your local Granola cache. No data is sent anywhere except to Claude during your conversation. The cache file never leaves your machine.

## What is a Skill?

[Skills](https://docs.claude.com/en/docs/claude-code/skills) are folders of instructions and scripts that Claude can discover and use to perform tasks more effectively. When you ask about meetings, Claude discovers this skill, loads the instructions, executes the query script, and presents the results.

## Contributing

Contributions are welcome! Feel free to open issues or submit pull requests.

## Learn More

- [Granola](https://granola.ai) - The AI notepad for meetings
- [Claude Code Skills Documentation](https://docs.claude.com/en/docs/claude-code/skills)
- [GitHub Issues](https://github.com/asterlabs-ai/querying-granola/issues)

## License

MIT License - see [LICENSE](LICENSE) file for details.
