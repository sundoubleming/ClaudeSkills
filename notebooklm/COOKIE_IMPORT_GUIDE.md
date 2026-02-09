# Cookie Import Authentication Guide

## 🎯 Overview

This guide shows you how to authenticate NotebookLM skill **without opening a browser on the server**. Perfect for headless servers, remote machines, or when you want to authenticate from a different device.

## 🚀 Quick Start

### Method 1: Interactive Mode (Recommended)

```bash
python scripts/run.py auth_manager.py import-cookies
```

Follow the on-screen instructions to:
1. Install a browser extension
2. Login to NotebookLM
3. Export cookies
4. Paste them into the terminal

### Method 2: From File

```bash
# Export cookies to a file on your local machine
# Then upload the file to the server and run:
python scripts/run.py auth_manager.py import-cookies --file cookies.json
```

### Method 3: From Command Line

```bash
python scripts/run.py auth_manager.py import-cookies --json '[{"name":"...","value":"..."}]'
```

---

## 📋 Detailed Instructions

### Step 1: Install Browser Extension

Choose one of these extensions on **your local machine** (not the server):

#### Chrome/Edge:
- **EditThisCookie**: https://chrome.google.com/webstore
  - Search: "EditThisCookie"
  - Install the extension

- **Cookie-Editor**: https://chrome.google.com/webstore
  - Search: "Cookie-Editor"
  - Install the extension

#### Firefox:
- **Cookie-Editor**: https://addons.mozilla.org
  - Search: "Cookie-Editor"
  - Install the extension

### Step 2: Login to NotebookLM

1. Open your browser and visit: **https://notebooklm.google.com**
2. Login with your Google account
3. Make sure you can see your notebooks (verify you're logged in)

### Step 3: Export Cookies

#### Using EditThisCookie:

1. Click the EditThisCookie icon in your browser toolbar
2. Click the "Export" button (looks like a download icon)
3. Cookies are copied to your clipboard as JSON

#### Using Cookie-Editor:

1. Click the Cookie-Editor icon in your browser toolbar
2. Click "Export" at the bottom
3. Choose "JSON" format
4. Click "Copy to clipboard"

### Step 4: Import to Server

#### Option A: Interactive Mode

```bash
# On your server
cd ~/.claude/skills/notebooklm
python scripts/run.py auth_manager.py import-cookies
```

When prompted:
1. Paste the JSON you copied
2. Press Enter
3. Type `END` on a new line
4. Press Enter again

#### Option B: Via File

```bash
# On your local machine, save cookies to file
echo '[paste cookies here]' > cookies.json

# Upload to server
scp cookies.json user@server:~/.claude/skills/notebooklm/

# On server, import
cd ~/.claude/skills/notebooklm
python scripts/run.py auth_manager.py import-cookies --file cookies.json

# Clean up
rm cookies.json
```

---

## 🔍 Verification

After importing, verify authentication works:

```bash
python scripts/run.py auth_manager.py status
python scripts/run.py auth_manager.py validate
```

You should see:
```
✅ Authentication is valid
```

---

## 💡 Usage Examples

### Example 1: Complete Workflow

```bash
# 1. On your local PC: Visit notebooklm.google.com and login
# 2. Export cookies using browser extension
# 3. On server:
python scripts/run.py auth_manager.py import-cookies
# [Paste cookies when prompted]

# 4. Verify
python scripts/run.py auth_manager.py status

# 5. Start querying
python scripts/run.py ask_question.py \
  --question "What is the main topic of this notebook?" \
  --notebook-url "https://notebooklm.google.com/notebook/..."
```

### Example 2: Automated Script

```bash
#!/bin/bash
# authenticate.sh

# Assuming cookies.json is already on the server
cd ~/.claude/skills/notebooklm

# Import cookies
python scripts/run.py auth_manager.py import-cookies --file cookies.json

# Validate
if python scripts/run.py auth_manager.py validate; then
    echo "✅ Authentication successful"
    rm cookies.json  # Clean up
else
    echo "❌ Authentication failed"
    exit 1
fi
```

---

## 🔧 Troubleshooting

### "No Google cookies found"

**Problem**: The exported cookies don't contain Google authentication cookies.

**Solution**:
1. Make sure you're logged in to NotebookLM
2. Export cookies while on the `notebooklm.google.com` page
3. Don't export from a different Google page

### "Invalid JSON format"

**Problem**: The pasted text is not valid JSON.

**Solution**:
1. Make sure you copied the entire JSON output
2. Don't add extra text before or after the JSON
3. Try using the `--file` method instead

### "Authentication is invalid"

**Problem**: Cookies were imported but don't work.

**Solution**:
1. Make sure you exported cookies from the same Google account
2. Try exporting cookies again (they may have expired)
3. Make sure you're logged in to NotebookLM before exporting

### Cookies expire quickly

**Problem**: Authentication works but expires after a few hours.

**Solution**:
1. This is normal for Google session cookies
2. Set up a cron job to re-import cookies periodically
3. Or use the original `setup` method on a machine with a desktop

---

## 🔐 Security Notes

### Safe Practices:

✅ **DO**:
- Use a dedicated Google account for automation
- Delete cookie files after importing
- Use SSH tunnels when transferring cookies
- Rotate cookies regularly

❌ **DON'T**:
- Share cookie files with others
- Commit cookie files to git
- Store cookies in plain text long-term
- Use your personal Google account

### Secure Transfer:

```bash
# Use SCP with SSH key
scp -i ~/.ssh/id_rsa cookies.json user@server:~/

# Or use SSH pipe (no file on disk)
cat cookies.json | ssh user@server 'cd ~/.claude/skills/notebooklm && python scripts/run.py auth_manager.py import-cookies --json "$(cat)"'
```

---

## 📊 Comparison: Cookie Import vs Browser Setup

| Feature | Cookie Import | Browser Setup |
|---------|--------------|---------------|
| **Requires Desktop** | ❌ No | ✅ Yes |
| **Works on Headless Server** | ✅ Yes | ❌ No |
| **Setup Complexity** | 🟡 Medium | 🟢 Simple |
| **Security** | 🟡 Manual transfer | 🟢 Direct |
| **Best For** | Remote servers | Local machines |

---

## 🎓 Advanced Usage

### Automated Cookie Refresh

```bash
#!/bin/bash
# refresh-auth.sh - Run this on your local machine

# Export cookies using browser extension
# This assumes you have a way to programmatically export cookies
# (e.g., using a browser automation tool)

# Upload to server
scp cookies.json server:~/.claude/skills/notebooklm/

# Import on server
ssh server 'cd ~/.claude/skills/notebooklm && python scripts/run.py auth_manager.py import-cookies --file cookies.json && rm cookies.json'

echo "✅ Authentication refreshed"
```

### Multiple Accounts

```bash
# Save cookies for different accounts
python scripts/run.py auth_manager.py import-cookies --file account1.json
# Use NotebookLM...

# Switch accounts
python scripts/run.py auth_manager.py clear
python scripts/run.py auth_manager.py import-cookies --file account2.json
```

---

## 📚 Related Commands

```bash
# Check authentication status
python scripts/run.py auth_manager.py status

# Validate authentication works
python scripts/run.py auth_manager.py validate

# Clear authentication
python scripts/run.py auth_manager.py clear

# Re-authenticate (browser method)
python scripts/run.py auth_manager.py reauth
```

---

## 🆘 Getting Help

If you encounter issues:

1. Check authentication status:
   ```bash
   python scripts/run.py auth_manager.py status
   ```

2. Validate cookies:
   ```bash
   python scripts/run.py auth_manager.py validate
   ```

3. Try re-importing:
   ```bash
   python scripts/run.py auth_manager.py clear
   python scripts/run.py auth_manager.py import-cookies
   ```

4. Check the troubleshooting guide:
   ```bash
   cat references/troubleshooting.md
   ```

---

## ✅ Success Checklist

- [ ] Browser extension installed
- [ ] Logged in to NotebookLM
- [ ] Cookies exported successfully
- [ ] Cookies imported to server
- [ ] Authentication validated
- [ ] First query successful
- [ ] Cookie file deleted (security)

---

**You're all set!** You can now use NotebookLM skill on your headless server without ever opening a browser on it.
