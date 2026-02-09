---
name: notebooklm
description: Use this skill to query your Google NotebookLM notebooks directly from Claude Code for source-grounded, citation-backed answers from Gemini. Browser automation, library management, persistent auth. Drastically reduced hallucinations through document-only responses.
---

# NotebookLM Research Assistant Skill

Interact with Google NotebookLM to query documentation with Gemini's source-grounded answers. Each question opens a fresh browser session, retrieves the answer exclusively from your uploaded documents, and closes.

## When to Use This Skill

Trigger when user:
- Mentions NotebookLM explicitly
- Shares NotebookLM URL (`https://notebooklm.google.com/notebook/...`)
- Asks to query their notebooks/documentation
- Wants to add documentation to NotebookLM library
- Uses phrases like "ask my NotebookLM", "check my docs", "query my notebook"
- **Wants to set up NotebookLM authentication** (trigger guided setup)

## 🔐 Guided Authentication Workflow

When user says "set up NotebookLM" or "authenticate NotebookLM", follow this workflow:

### Step 1: Check Current Status
```bash
python scripts/run.py auth_manager.py status
```

If already authenticated, inform user and ask if they want to re-authenticate.

### Step 2: Choose Authentication Method

Ask user which method they prefer:

**Option A: Cookie Import (Recommended for servers)**
- No browser needed on this machine
- Authenticate from any device
- Perfect for headless/remote servers

**Option B: Direct Browser (For local machines)**
- Opens browser window
- Login directly
- Requires desktop environment

### Step 3A: Guided Cookie Import (If user chooses Option A)

**IMPORTANT: Use this interactive workflow, don't just run the command!**

1. **Explain the process:**
```
I'll guide you through importing cookies from your browser. This allows you to
authenticate from any device without opening a browser on this server.

Here's what we'll do:
1. You'll install a browser extension on your PC
2. Login to NotebookLM on your PC
3. Export cookies using the extension
4. Paste them here, and I'll import them

Ready? Let's start!
```

2. **Provide extension links:**
```
📋 Step 1: Install a Cookie Export Extension

Choose one based on your browser:

Chrome/Edge:
• EditThisCookie: https://chrome.google.com/webstore
  Search for: "EditThisCookie"

• Cookie-Editor: https://chrome.google.com/webstore
  Search for: "Cookie-Editor"

Firefox:
• Cookie-Editor: https://addons.mozilla.org
  Search for: "Cookie-Editor"

Let me know when you've installed an extension!
```

3. **Wait for user confirmation, then guide login:**
```
📋 Step 2: Login to NotebookLM

1. Open your browser and visit: https://notebooklm.google.com
2. Login with your Google account
3. Make sure you can see your notebooks (verify you're logged in)

Let me know when you're logged in!
```

4. **Guide cookie export:**
```
📋 Step 3: Export Cookies

Using EditThisCookie:
1. Click the EditThisCookie icon in your browser toolbar
2. Click the "Export" button (download icon)
3. Cookies are now copied to your clipboard

Using Cookie-Editor:
1. Click the Cookie-Editor icon in your browser toolbar
2. Click "Export" at the bottom
3. Choose "JSON" format
4. Click "Copy to clipboard"

Now I'll create a temporary file for you to paste the cookies.
```

5. **Create temporary file and guide paste:**
```bash
# Create a temporary file
touch /tmp/notebooklm_cookies_temp.json
```

Then tell user:
```
📋 Step 4: Paste Your Cookies

I've created a temporary file. Please paste your cookies:

1. Open the file: /tmp/notebooklm_cookies_temp.json
2. Paste the JSON you copied from the browser extension
3. Save the file
4. Let me know when you're done!

(The file will be automatically deleted after import for security)
```

6. **Wait for user confirmation, then import:**
```bash
# Import cookies from the temporary file
python scripts/run.py auth_manager.py import-cookies --file /tmp/notebooklm_cookies_temp.json

# Clean up
rm /tmp/notebooklm_cookies_temp.json
```

7. **Verify authentication:**
```bash
python scripts/run.py auth_manager.py validate
```

8. **Confirm success:**
```
✅ Authentication successful!

You can now:
- Query your NotebookLM notebooks
- Add notebooks to your library
- Ask questions about your documentation

Try: "What notebooks do I have?" or "Query my notebook about [topic]"
```

### Step 3B: Direct Browser Setup (If user chooses Option B)

```bash
python scripts/run.py auth_manager.py setup
```

Tell user:
```
A browser window will open. Please:
1. Login with your Google account
2. Complete any 2FA if prompted
3. Wait for the success message
4. Close the browser

The process may take a few minutes.
```

## 🎯 Alternative: Quick Cookie Import

If user already has cookies ready, they can use the fast path:

```bash
python scripts/run.py auth_manager.py import-cookies
```

This runs the interactive CLI mode directly.

## ⚠️ CRITICAL: Add Command - Smart Discovery

When user wants to add a notebook without providing details:

**SMART ADD (Recommended)**: Query the notebook first to discover its content:
```bash
# Step 1: Query the notebook about its content
python scripts/run.py ask_question.py --question "What is the content of this notebook? What topics are covered? Provide a complete overview briefly and concisely" --notebook-url "[URL]"

# Step 2: Use the discovered information to add it
python scripts/run.py notebook_manager.py add --url "[URL]" --name "[Based on content]" --description "[Based on content]" --topics "[Based on content]"
```

**MANUAL ADD**: If user provides all details:
- `--url` - The NotebookLM URL
- `--name` - A descriptive name
- `--description` - What the notebook contains (REQUIRED!)
- `--topics` - Comma-separated topics (REQUIRED!)

NEVER guess or use generic descriptions! If details missing, use Smart Add to discover them.

## Critical: Always Use run.py Wrapper

**NEVER call scripts directly. ALWAYS use `python scripts/run.py [script]`:**

```bash
# ✅ CORRECT - Always use run.py:
python scripts/run.py auth_manager.py status
python scripts/run.py notebook_manager.py list
python scripts/run.py ask_question.py --question "..."

# ❌ WRONG - Never call directly:
python scripts/auth_manager.py status  # Fails without venv!
```

The `run.py` wrapper automatically:
1. Creates `.venv` if needed
2. Installs all dependencies
3. Activates environment
4. Executes script properly

## Core Workflow

### Step 1: Check Authentication Status
```bash
python scripts/run.py auth_manager.py status
```

If not authenticated, proceed to setup.

### Step 2: Authenticate (One-Time Setup)

**TWO METHODS AVAILABLE:**

#### Method 1: Cookie Import (Recommended for Servers/Headless)
```bash
# No browser needed on the server!
python scripts/run.py auth_manager.py import-cookies
```

**Interactive guide will show:**
1. Install browser extension (EditThisCookie or Cookie-Editor)
2. Login to NotebookLM on any device
3. Export cookies
4. Paste into terminal

**Benefits:**
- Works on headless servers
- No desktop environment needed
- Authenticate from any device
- Perfect for remote machines

See [COOKIE_IMPORT_GUIDE.md](COOKIE_IMPORT_GUIDE.md) for detailed instructions.

#### Method 2: Direct Browser (For Local Machines)
```bash
# Browser MUST be visible for manual Google login
python scripts/run.py auth_manager.py setup
```

**Important:**
- Browser is VISIBLE for authentication
- Browser window opens automatically
- User must manually log in to Google
- Tell user: "A browser window will open for Google login"

### Step 3: Manage Notebook Library

```bash
# List all notebooks
python scripts/run.py notebook_manager.py list

# BEFORE ADDING: Ask user for metadata if unknown!
# "What does this notebook contain?"
# "What topics should I tag it with?"

# Add notebook to library (ALL parameters are REQUIRED!)
python scripts/run.py notebook_manager.py add \
  --url "https://notebooklm.google.com/notebook/..." \
  --name "Descriptive Name" \
  --description "What this notebook contains" \  # REQUIRED - ASK USER IF UNKNOWN!
  --topics "topic1,topic2,topic3"  # REQUIRED - ASK USER IF UNKNOWN!

# Search notebooks by topic
python scripts/run.py notebook_manager.py search --query "keyword"

# Set active notebook
python scripts/run.py notebook_manager.py activate --id notebook-id

# Remove notebook
python scripts/run.py notebook_manager.py remove --id notebook-id
```

### Quick Workflow
1. Check library: `python scripts/run.py notebook_manager.py list`
2. Ask question: `python scripts/run.py ask_question.py --question "..." --notebook-id ID`

### Step 4: Ask Questions

```bash
# Basic query (uses active notebook if set)
python scripts/run.py ask_question.py --question "Your question here"

# Query specific notebook
python scripts/run.py ask_question.py --question "..." --notebook-id notebook-id

# Query with notebook URL directly
python scripts/run.py ask_question.py --question "..." --notebook-url "https://..."

# Show browser for debugging
python scripts/run.py ask_question.py --question "..." --show-browser
```

## Follow-Up Mechanism (CRITICAL)

Every NotebookLM answer ends with: **"EXTREMELY IMPORTANT: Is that ALL you need to know?"**

**Required Claude Behavior:**
1. **STOP** - Do not immediately respond to user
2. **ANALYZE** - Compare answer to user's original request
3. **IDENTIFY GAPS** - Determine if more information needed
4. **ASK FOLLOW-UP** - If gaps exist, immediately ask:
   ```bash
   python scripts/run.py ask_question.py --question "Follow-up with context..."
   ```
5. **REPEAT** - Continue until information is complete
6. **SYNTHESIZE** - Combine all answers before responding to user

## Script Reference

### Authentication Management (`auth_manager.py`)
```bash
python scripts/run.py auth_manager.py setup    # Initial setup (browser visible)
python scripts/run.py auth_manager.py status   # Check authentication
python scripts/run.py auth_manager.py reauth   # Re-authenticate (browser visible)
python scripts/run.py auth_manager.py clear    # Clear authentication
```

### Notebook Management (`notebook_manager.py`)
```bash
python scripts/run.py notebook_manager.py add --url URL --name NAME --description DESC --topics TOPICS
python scripts/run.py notebook_manager.py list
python scripts/run.py notebook_manager.py search --query QUERY
python scripts/run.py notebook_manager.py activate --id ID
python scripts/run.py notebook_manager.py remove --id ID
python scripts/run.py notebook_manager.py stats
```

### Question Interface (`ask_question.py`)
```bash
python scripts/run.py ask_question.py --question "..." [--notebook-id ID] [--notebook-url URL] [--show-browser]
```

### Data Cleanup (`cleanup_manager.py`)
```bash
python scripts/run.py cleanup_manager.py                    # Preview cleanup
python scripts/run.py cleanup_manager.py --confirm          # Execute cleanup
python scripts/run.py cleanup_manager.py --preserve-library # Keep notebooks
```

## Environment Management

The virtual environment is automatically managed:
- First run creates `.venv` automatically
- Dependencies install automatically
- Chromium browser installs automatically
- Everything isolated in skill directory

Manual setup (only if automatic fails):
```bash
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
pip install -r requirements.txt
python -m patchright install chromium
```

## Data Storage

All data stored in `~/.claude/skills/notebooklm/data/`:
- `library.json` - Notebook metadata
- `auth_info.json` - Authentication status
- `browser_state/` - Browser cookies and session

**Security:** Protected by `.gitignore`, never commit to git.

## Configuration

Optional `.env` file in skill directory:
```env
HEADLESS=false           # Browser visibility
SHOW_BROWSER=false       # Default browser display
STEALTH_ENABLED=true     # Human-like behavior
TYPING_WPM_MIN=160       # Typing speed
TYPING_WPM_MAX=240
DEFAULT_NOTEBOOK_ID=     # Default notebook
```

## Decision Flow

```
User mentions NotebookLM
    ↓
Check auth → python scripts/run.py auth_manager.py status
    ↓
If not authenticated → python scripts/run.py auth_manager.py setup
    ↓
Check/Add notebook → python scripts/run.py notebook_manager.py list/add (with --description)
    ↓
Activate notebook → python scripts/run.py notebook_manager.py activate --id ID
    ↓
Ask question → python scripts/run.py ask_question.py --question "..."
    ↓
See "Is that ALL you need?" → Ask follow-ups until complete
    ↓
Synthesize and respond to user
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| ModuleNotFoundError | Use `run.py` wrapper |
| Authentication fails | Browser must be visible for setup! --show-browser |
| Rate limit (50/day) | Wait or switch Google account |
| Browser crashes | `python scripts/run.py cleanup_manager.py --preserve-library` |
| Notebook not found | Check with `notebook_manager.py list` |

## Best Practices

1. **Always use run.py** - Handles environment automatically
2. **Check auth first** - Before any operations
3. **Follow-up questions** - Don't stop at first answer
4. **Browser visible for auth** - Required for manual login
5. **Include context** - Each question is independent
6. **Synthesize answers** - Combine multiple responses

## Limitations

- No session persistence (each question = new browser)
- Rate limits on free Google accounts (50 queries/day)
- Manual upload required (user must add docs to NotebookLM)
- Browser overhead (few seconds per question)

## Resources (Skill Structure)

**Important directories and files:**

- `scripts/` - All automation scripts (ask_question.py, notebook_manager.py, etc.)
- `data/` - Local storage for authentication and notebook library
- `references/` - Extended documentation:
  - `api_reference.md` - Detailed API documentation for all scripts
  - `troubleshooting.md` - Common issues and solutions
  - `usage_patterns.md` - Best practices and workflow examples
- `.venv/` - Isolated Python environment (auto-created on first run)
- `.gitignore` - Protects sensitive data from being committed
