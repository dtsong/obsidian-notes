# Obsidian Vault Setup Guide

## Folder Structure

```
📁 Your Vault/
├── 📁 00-Inbox/                    # Quick capture, unsorted notes
├── 📁 01-Daily/                    # Daily notes (auto-generated)
├── 📁 02-Career/
│   ├── 📁 Brag-Doc/                # Win entries, accomplishments
│   ├── 📁 Growth-Roadmap/          # Learning paths, skill development
│   ├── 📁 Reviews/                 # Weekly/monthly/quarterly summaries
│   └── 📁 Harness/                 # Company-specific notes, onboarding
├── 📁 03-Projects/
│   ├── 📁 AI-Projects/             # Individual AI project folders
│   └── 📁 Claude-Updates/          # For Claude Code integration
├── 📁 04-Pokemon-TCG/
│   ├── 📁 Decks/                   # Deck builds and analysis
│   ├── 📁 Events/                  # Event planning and logs
│   └── 📁 Automation/              # Workflow automation ideas
├── 📁 05-Resources/                # Reference material, links, snippets
├── 📁 Templates/                   # Your template files (see below)
└── 📁 Attachments/                 # Images, PDFs, etc.
```

## How to Import This Structure

### Option 1: Manual Creation
1. Open your Obsidian vault
2. Right-click in the file explorer → "New folder"
3. Create each folder following the structure above

### Option 2: Use the Included Script
If you have a terminal/command line:

```bash
# Navigate to your vault location first
cd /path/to/your/vault

# Create all folders
mkdir -p 00-Inbox 01-Daily 02-Career/{Brag-Doc,Growth-Roadmap,Reviews,Harness} 03-Projects/{AI-Projects,Claude-Updates} 04-Pokemon-TCG/{Decks,Events,Automation} 05-Resources Templates Attachments
```

### Option 3: Copy the Template Files
1. Download the template files from this starter kit
2. Copy the `Templates/` folder contents into your vault's `Templates/` folder
3. Obsidian will recognize them automatically

---

## Plugin Installation

### Enable Community Plugins
1. Open Obsidian Settings (gear icon or `Cmd/Ctrl + ,`)
2. Go to **Community plugins** in the left sidebar
3. Click **Turn on community plugins** (if not already enabled)
4. Click **Browse**

### Recommended Plugins (Install in This Order)

#### Core Workflow
| Plugin | Purpose | Install Priority |
|--------|---------|------------------|
| **Calendar** | Visual calendar for daily notes | 1 - Install first |
| **Templater** | Advanced templates with variables | 2 - Essential |
| **Dataview** | Query notes like a database | 3 - Essential |
| **Tasks** | Track todos across vault | 4 - High |
| **Periodic Notes** | Weekly/monthly review notes | 5 - High |

#### Productivity Boosters
| Plugin | Purpose | Install Priority |
|--------|---------|------------------|
| **QuickAdd** | Fast capture with templates | 6 - Medium |
| **Homepage** | Dashboard when opening vault | 7 - Medium |
| **Kanban** | Visual project boards | 8 - Optional |

#### Integration & Backup
| Plugin | Purpose | Install Priority |
|--------|---------|------------------|
| **Obsidian Git** | Version control, backup | 9 - Recommended |
| **Excalidraw** | Visual diagrams | 10 - Optional |

### Plugin Configuration

#### Templater Setup
1. Settings → Templater → Template folder location: `Templates`
2. Enable "Trigger Templater on new file creation"
3. Enable "Enable folder templates" and map:
   - `01-Daily` → `Templates/Daily-Note.md`
   - `02-Career/Brag-Doc` → `Templates/Brag-Entry.md`
   - `03-Projects/AI-Projects` → `Templates/Project.md`

#### Daily Notes Setup
1. Settings → Daily notes (core plugin, enable if needed)
2. New file location: `01-Daily`
3. Template file location: `Templates/Daily-Note.md`
4. Date format: `YYYY-MM-DD`

#### Periodic Notes Setup
1. Settings → Periodic Notes
2. Enable Weekly Notes
3. Weekly note folder: `02-Career/Reviews`
4. Weekly template: `Templates/Weekly-Review.md`

---

## Claude Code Integration

Your vault is just a folder of markdown files. To let Claude Code work with it:

### Setup
1. Know your vault path (e.g., `~/Documents/Obsidian/MyVault/`)
2. When using Claude Code, reference files directly:
   ```
   claude "Add a new entry to my brag doc at ~/Documents/Obsidian/MyVault/02-Career/Brag-Doc/"
   ```

### Conventions for Claude Code
- Claude Code should write to `03-Projects/Claude-Updates/` for logs
- Use consistent frontmatter so Dataview can query Claude-generated notes
- Include `source: claude-code` in frontmatter for tracking

### Example Claude Code Commands
```bash
# Have Claude log work on a project
claude "Log today's progress on the RAG project to my Obsidian vault"

# Generate a summary
claude "Summarize this week's entries in my brag doc"
```

---

## Quick Start Checklist

- [ ] Create folder structure (or run the script)
- [ ] Copy template files to `Templates/`
- [ ] Install plugins in order listed above
- [ ] Configure Templater folder mapping
- [ ] Configure Daily Notes location
- [ ] Create your first daily note
- [ ] Log your first brag doc entry
- [ ] Create your first project note
