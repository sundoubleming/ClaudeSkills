# 🍪 Quick Start: Cookie Import Authentication

## For Headless Servers / Remote Machines

### 🚀 5-Minute Setup

```bash
# On your server
cd ~/.claude/skills/notebooklm
python scripts/run.py auth_manager.py import-cookies
```

### 📋 Follow These Steps:

#### 1. Install Browser Extension (On Your PC)
- **Chrome/Edge**: Install "EditThisCookie" or "Cookie-Editor"
- **Firefox**: Install "Cookie-Editor"

#### 2. Login to NotebookLM (On Your PC)
- Visit: https://notebooklm.google.com
- Login with your Google account
- Verify you can see your notebooks

#### 3. Export Cookies (On Your PC)
- Click the extension icon
- Click "Export" button
- Cookies copied to clipboard as JSON

#### 4. Import to Server
- Paste the JSON when prompted
- Press Enter
- Type `END` on a new line
- Press Enter again

#### 5. Done!
```bash
# Verify authentication
python scripts/run.py auth_manager.py status

# Start querying
python scripts/run.py ask_question.py \
  --question "What is this notebook about?" \
  --notebook-url "https://notebooklm.google.com/notebook/..."
```

---

## 🎯 Why Cookie Import?

| Feature | Cookie Import | Browser Setup |
|---------|--------------|---------------|
| Works on headless servers | ✅ Yes | ❌ No |
| Requires desktop | ❌ No | ✅ Yes |
| Authenticate from any device | ✅ Yes | ❌ No |
| Setup complexity | 🟡 Medium | 🟢 Simple |

---

## 🔧 Alternative Methods

### From File
```bash
# Save cookies to file on your PC
echo '[paste cookies]' > cookies.json

# Upload to server
scp cookies.json user@server:~/

# Import on server
python scripts/run.py auth_manager.py import-cookies --file cookies.json
```

### From Command Line
```bash
python scripts/run.py auth_manager.py import-cookies --json '[{"name":"..."}]'
```

---

## 📖 Full Documentation

- [Complete Cookie Import Guide](COOKIE_IMPORT_GUIDE.md)
- [Main README](README.md)
- [Skill Documentation](SKILL.md)

---

## ❓ Troubleshooting

### "No Google cookies found"
- Make sure you're on notebooklm.google.com when exporting
- Verify you're logged in before exporting

### "Invalid JSON format"
- Copy the entire JSON output
- Don't add extra text before/after

### "Authentication is invalid"
- Cookies may have expired, export again
- Make sure you're using the same Google account

---

## 🔐 Security Tips

✅ **DO**:
- Use a dedicated Google account
- Delete cookie files after importing
- Use SSH tunnels for transfer

❌ **DON'T**:
- Share cookie files
- Commit cookies to git
- Store cookies long-term

---

**Need help?** See [COOKIE_IMPORT_GUIDE.md](COOKIE_IMPORT_GUIDE.md) for detailed instructions.
