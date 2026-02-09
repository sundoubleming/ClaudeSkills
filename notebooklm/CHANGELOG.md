# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.4.0] - 2026-02-10

### Added
- **Cookie Import Authentication** - New authentication method for headless servers
  - `auth_manager.py import-cookies` command for browser-free authentication
  - Interactive mode with step-by-step guidance
  - File import mode: `--file cookies.json`
  - Command-line mode: `--json '[...]'`
  - Perfect for remote servers without desktop environments
  - Supports multiple browser extension formats (EditThisCookie, Cookie-Editor)
  - Smart cookie format detection and conversion
  - Comprehensive documentation in `COOKIE_IMPORT_GUIDE.md`
  - Quick reference in `QUICK_START_COOKIE_IMPORT.md`

### Changed
- **README.md** - Updated authentication section with two methods
  - Added cookie import as recommended method for servers
  - Added comparison table between authentication methods
- **SKILL.md** - Enhanced Step 2 with dual authentication paths
  - Cookie import for headless/remote scenarios
  - Direct browser for local machines

### Documentation
- New `COOKIE_IMPORT_GUIDE.md` - Complete guide with troubleshooting
- New `QUICK_START_COOKIE_IMPORT.md` - 5-minute quick start
- New `IMPLEMENTATION_SUMMARY.md` - Technical implementation details

## [1.3.0] - 2025-11-21

### Added
- **Modular Architecture** - Refactored codebase for better maintainability
  - New `config.py` - Centralized configuration (paths, selectors, timeouts)
  - New `browser_utils.py` - BrowserFactory and StealthUtils classes
  - Cleaner separation of concerns across all scripts

### Changed
- **Timeout increased to 120 seconds** - Long queries no longer timeout prematurely
  - `ask_question.py`: 30s → 120s
  - `browser_session.py`: 30s → 120s
  - Resolves Issue #4

### Fixed
- **Thinking Message Detection** - Fixed incomplete answers showing placeholder text
  - Now waits for `div.thinking-message` element to disappear before reading answer
  - Answers like "Reviewing the content..." or "Looking for answers..." no longer returned prematurely
  - Works reliably across all languages and NotebookLM UI changes

- **Correct CSS Selectors** - Updated to match current NotebookLM UI
  - Changed from `.response-content, .message-content` to `.to-user-container .message-text-content`
  - Consistent selectors across all scripts

- **Stability Detection** - Improved answer completeness check
  - Now requires 3 consecutive stable polls instead of 1 second wait
  - Prevents truncated responses during streaming

## [1.2.0] - 2025-10-28

### Added
- Initial public release
- NotebookLM integration via browser automation
- Session-based conversations with Gemini 2.5
- Notebook library management
- Knowledge base preparation tools
- Google authentication with persistent sessions
