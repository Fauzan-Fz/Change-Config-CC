# change-cc

> **Claude Code Model & Settings Switcher** — A bash script to easily manage Claude Code models, API endpoints, and authentication tokens via `~/.claude/settings.json`.

[![Bash](https://img.shields.io/badge/Bash-4.0%2B-green.svg)](https://www.gnu.org/software/bash/)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Compatible-blue.svg)](https://claude.ai/code)

---

## 📋 Overview

`change-cc` is an interactive CLI tool that lets you switch Claude Code models, change API endpoints, update authentication tokens, and manage configuration backups — all without manually editing JSON files.

### ✨ Features

| Feature | Description |
|---------|-------------|
| **🔄 Model Switching** | Change main model and all variants (Opus, Sonnet, Haiku, Fable, Small/Fast) |
| **📏 Context Window** | Set context size per model: `model[500k]`, `model[1m]`, `model[2m]`, etc. |
| **🌐 Endpoint Management** | Switch between local proxy, official Anthropic API, or custom endpoints |
| **🔑 Auth Token** | Update `ANTHROPIC_AUTH_TOKEN` securely |
| **💾 Backup/Restore** | Auto-backup before changes, manual backup, and restore from any backup |
| **🎨 Interactive Menu** | Color-coded, user-friendly navigation with fixed layout consistency |
| **⚡ Direct Mode** | `change-cc "model-name[1m]"` for quick model updates |
| **📦 Global Install** | Install to `~/.local/bin` for system-wide access |

---

## 🚀 Installation

### Quick Install (Recommended)

```bash
# Clone and install globally
git clone https://github.com/Fauzan-Fz/Change-Config-CC.git
cd Change-Config-CC
chmod +x change-cc
./change-cc --install
```

This installs to `~/.local/bin/change-cc`. Ensure `~/.local/bin` is in your `PATH`.

### Manual Install

```bash
# Copy to any directory in PATH
cp change-cc ~/.local/bin/
chmod +x ~/.local/bin/change-cc
```

### Uninstall

```bash
./change-cc --uninstall
# or
change-cc --uninstall
```

---

## 📖 Usage

### Interactive Menu (Default)

```bash
change-cc
# or
./change-cc
```

**Menu Options:**
```
══════════════════════════════════════════════════════════════
                    CHANGE-CC MENU                             
══════════════════════════════════════════════════════════════
  1) Change Endpoint URL (ANTHROPIC_BASE_URL)
  2) Change Model Context (model[500k], model[1m])
  3) Change Model Variants (Default, Opus, Sonnet, Haiku, Fable, etc.)
  4) Change Main Model Field (.model)
  5) Change API Key (ANTHROPIC_AUTH_TOKEN)
  6) Backup Menu (make / restore backup)
  0) Exit
```

### Direct Model Setting

```bash
# Set main model with context
change-cc "anthropic/claude-sonnet[1m]"

# Set model without context (uses model default)
change-cc "anthropic/claude-sonnet"

# Any valid model identifier
change-cc "anthropic/claude-opus[200k]"
```

### List Current Configuration

```bash
change-cc --list
# or
change-cc -l
```

**Output:**
```
╔══════════════════════════════════════════════════════════════╗
║           Current Claude Code Configuration                 ║
╚══════════════════════════════════════════════════════════════╝

🌐 Base URL (ANTHROPIC_BASE_URL):   https://api.anthropic.com
🔑 Auth Token:                      sk-xxxxxxxxxxxx-...
🤖 Main Model (model field):        haiku

Model Variants:
  Default Model                  = anthropic/claude-sonnet
  Small/Fast Model               = anthropic/claude-haiku
  Opus Model                     = anthropic/claude-opus
  Sonnet Model                   = anthropic/claude-sonnet
  Haiku Model                    = anthropic/claude-haiku
  Fable Model                    = anthropic/claude-fable
```

### Help

```bash
change-cc --help
# or
change-cc -h
```

---

## ⌨️ Menu Walkthrough

### 1. Change Endpoint URL (ANTHROPIC_BASE_URL)

```
═══ Change Endpoint URL (ANTHROPIC_BASE_URL) ═══
Current: https://api.anthropic.com
Options:
  1) Use current from settings.json (https://api.anthropic.com)
  2) https://api.anthropic.com (official Anthropic)
  3) Custom URL
  0) Exit
```

### 2. Change Model Context

Select a model variant from guaranteed fixed order, then choose context size:

```
═══ Change Model Variant Context ═══
Select which model variant to add/change context (Guaranteed Fixed Order):
   1) Default Model (ANTHROPIC_MODEL) = anthropic/claude-sonnet
   2) Small/Fast Model (ANTHROPIC_SMALL_FAST_MODEL) = anthropic/claude-haiku
   3) Opus Model (ANTHROPIC_DEFAULT_OPUS_MODEL) = anthropic/claude-opus
   ...
```

**Context Options:**
```
Select context suffix for model: anthropic/claude-haiku
   1)  [4k    ] (4k tokens)
   2)  [8k    ] (8k tokens)
   3)  [16k   ] (16k tokens)
   4)  [32k   ] (32k tokens)
   5)  [64k   ] (64k tokens)
   6)  [128k  ] (128k tokens)
   7)  [200k  ] (200k tokens)
   8)  [500k  ] (500k tokens)
   9)  [1m    ] (1m tokens)
  10)  [2m    ] (2m tokens)
  11)  Custom context (e.g., 500k, 1m)
  12)  Remove context (use model default)
  13)  Change model name (keep current context)
   0)  Back to variant selection
```

### 3. Change Model Variants

Modify any model variant directly with fixed layout consistency:

```
═══ Change Model Variants ═══
Current model variants (Guaranteed Fixed Layout):
   1) Default Model (ANTHROPIC_MODEL) = anthropic/claude-sonnet
   2) Small/Fast Model (ANTHROPIC_SMALL_FAST_MODEL) = anthropic/claude-haiku
   3) Opus Model (ANTHROPIC_DEFAULT_OPUS_MODEL) = anthropic/claude-opus
   4) Sonnet Model (ANTHROPIC_DEFAULT_SONNET_MODEL) = anthropic/claude-sonnet
   5) Haiku Model (ANTHROPIC_DEFAULT_HAIKU_MODEL) = anthropic/claude-haiku
   6) Fable Model (ANTHROPIC_DEFAULT_FABLE_MODEL) = anthropic/claude-fable
   7) Add/Edit Other Custom Model Variable
   0) Back to main menu
```

### 4. Change Main Model Field

Update top-level `.model` field (`sonnet`, `opus`, `haiku`, or custom).

### 5. Change API Key

```
═══ Change API Key (ANTHROPIC_AUTH_TOKEN) ═══
Current: sk-xxxxxxxxxxxxx-... (40 chars)
Enter new API key (input hidden):
```

### 6. Backup Menu

```
═══ Backup Menu ═══
  1) Make Backup (save current settings)
  2) Restore Backup (from list or custom path)
  0) Back to Main Menu
```

---

## 🎯 Context Window Format

The script uses bracket notation for context windows:

| Format | Tokens | Example |
|--------|--------|---------|
| `[4k]` | 4,000 | `model[4k]` |
| `[500k]` | 500,000 | `model[500k]` |
| `[1m]` | 1,000,000 | `model[1m]` |
| `[2m]` | 2,000,000 | `model[2m]` |
| *(none)* | Model default | `model` |

**Valid suffixes:** `k` (thousands), `m` (millions) — case insensitive.

---

## 📁 Configuration File

The script reads/writes to `~/.claude/settings.json`:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.anthropic.com",
    "ANTHROPIC_MODEL": "anthropic/claude-sonnet",
    "ANTHROPIC_SMALL_FAST_MODEL": "anthropic/claude-haiku",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "anthropic/claude-opus",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "anthropic/claude-sonnet",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "anthropic/claude-haiku",
    "ANTHROPIC_AUTH_TOKEN": "sk-...",
    "ANTHROPIC_DEFAULT_FABLE_MODEL": "anthropic/claude-fable"
  },
  "model": "haiku",
  "effortLevel": "high",
  "theme": "dark"
}
```

---

## 🔧 Requirements

- **Bash 4.0+**
- **jq** (JSON processor) — `apt install jq` / `brew install jq` / `pacman -S jq`
- **Claude Code** installed and configured

---

## 🤝 Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request
