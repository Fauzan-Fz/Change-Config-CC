# change-cc

> **Claude Code Model & Settings Switcher** — Bash script to switch Claude Code models, endpoints, and auth tokens via `~/.claude/settings.json`.

[![Bash](https://img.shields.io/badge/Bash-4.0%2B-green.svg)](https://www.gnu.org/software/bash/)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Compatible-blue.svg)](https://claude.ai/code)

Interactive CLI for Claude Code settings — change models, context windows, API endpoints, and auth tokens without editing JSON by hand. Auto-backup before every change.

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔄 Model Switching | Switch main model and all variants (Opus, Sonnet, Haiku, Fable, Small/Fast) |
| 📏 Context Window | Set per-model context: `model[500k]`, `model[1m]`, `model[2m]` |
| 🌐 Endpoint | Switch between local proxy, official Anthropic API, or custom `ANTHROPIC_BASE_URL` |
| 🔑 Auth Token | Update `ANTHROPIC_AUTH_TOKEN` securely (hidden input, masked display) |
| 💾 Backup / Restore | Auto-backup before every change + manual backup and restore |
| 🧹 Clean Backups | Delete with multi-select `2,3,4`, `all`, or `keep:5` |
| 🎨 Interactive Menu | Color-coded prompts with consistent layout |
| ⚡ Direct Mode | `change-cc "anthropic/claude-sonnet[1m]"` for quick updates |
| 🚀 Self-Update | Update to latest version from GitHub via menu or `change-cc --update` |

## 📦 Installation

### Quick Install (Recommended)

```bash
git clone https://github.com/Fauzan-Fz/Change-Config-CC.git
cd Change-Config-CC
chmod +x change-cc
./change-cc --install
```

Installs to `~/.local/bin/change-cc`. Make sure `~/.local/bin` is in your `PATH`:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
```

### Manual Install

```bash
cp change-cc ~/.local/bin/
chmod +x ~/.local/bin/change-cc
```

### Uninstall

```bash
change-cc --uninstall
# or
./change-cc --uninstall
```

## 🚀 Usage

### Interactive Menu

```bash
change-cc              # open menu
./change-cc            # from repo directory
```

```
1) Change Endpoint URL (ANTHROPIC_BASE_URL)
2) Change Model Context (model[500k], model[1m])
3) Change Model Variants (Default, Opus, Sonnet, Haiku, Fable...)
4) Change Main Model field (.model)
5) Change API Key (ANTHROPIC_AUTH_TOKEN)
6) Backup Menu (make / restore / clean)
7) Update Script (from GitHub)
0) Exit
```

### Direct Mode

```bash
change-cc "anthropic/claude-sonnet[1m]"   # set ANTHROPIC_MODEL with context
change-cc "anthropic/claude-sonnet"       # without context (model default)
change-cc "anthropic/claude-opus[200k]"
```

### Flags

```bash
change-cc -v / --version     # show version
change-cc -l / --list        # show current configuration
change-cc -U / --update      # update from GitHub
change-cc -h / --help        # help
```

**`--list` output:**

```
Base URL (ANTHROPIC_BASE_URL):  https://api.anthropic.com
Auth Token:                     «redacted:sk-…»
Main Model (model field):       haiku

Model Variants:
  Default Model     = anthropic/claude-sonnet
  Small/Fast Model  = anthropic/claude-haiku
  Opus Model        = anthropic/claude-opus
  Sonnet Model      = anthropic/claude-sonnet
  Haiku Model       = anthropic/claude-haiku
  Fable Model       = anthropic/claude-fable
```

## ⌨️ Menu Details

**1. Endpoint URL** — keep current, use official `https://api.anthropic.com`, or enter a custom URL.

**2. Model Context** — pick a variant (Default, Small/Fast, Opus, Sonnet, Haiku, Fable — fixed order), then choose context:

```
1) [4k]    2) [8k]   3) [16k]  4) [32k]  5) [64k]  6) [128k]
7) [200k]  8) [500k] 9) [1m]  10) [2m]  11) Custom  12) Remove context  13) Change model name
```

Context is appended as `model[1m]` and stored in `ANTHROPIC_*_MODEL`.

**3. Model Variants** — edit any variant directly or add a custom `ANTHROPIC_*` variable.

**4. Main Model Field** — update top-level `.model` (`sonnet`, `opus`, `haiku`, or custom).

**5. API Key** — enter new `ANTHROPIC_AUTH_TOKEN` (input hidden, current value masked).

**6. Backup Menu**

- `1) Make Backup` — save current `settings.json`
- `2) Restore Backup` — pick from list or enter custom path
- `3) Clean Backups` — `2,3,4` (specific), `all` (delete all), `keep:5` (keep latest 5)

**7. Update Script** — check GitHub for newer version, confirm before replacing.

## 🎯 Context Window

| Format | Tokens | Example |
|--------|--------|---------|
| `[4k]` | 4,000 | `model[4k]` |
| `[500k]` | 500,000 | `model[500k]` |
| `[1m]` | 1,000,000 | `model[1m]` |
| `[2m]` | 2,000,000 | `model[2m]` |
| *(none)* | Model default | `model` |

Valid suffixes: `k` (thousands), `m` (millions) — case insensitive. `Custom` accepts any `500k` / `1m` format.

## 📁 Configuration File

Reads and writes `~/.claude/settings.json`:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.anthropic.com",
    "ANTHROPIC_MODEL": "anthropic/claude-sonnet",
    "ANTHROPIC_SMALL_FAST_MODEL": "anthropic/claude-haiku",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "anthropic/claude-opus",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "anthropic/claude-sonnet",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "anthropic/claude-haiku",
    "ANTHROPIC_DEFAULT_FABLE_MODEL": "anthropic/claude-fable",
    "ANTHROPIC_AUTH_TOKEN": "sk-..."
  },
  "model": "haiku",
  "effortLevel": "high"
}
```

Backups are saved alongside it as `settings.json.backup.YYYYMMDD_HHMMSS` and created automatically before every change.

## 🔧 Requirements

- Bash 4.0+
- `jq` — `apt install jq` / `brew install jq` / `pacman -S jq`
- `curl` or `wget` (for self-update)
- Claude Code installed and configured

## 🔗 Related

- Python port (cross-platform, Windows/Linux/macOS): [Fauzan-Fz/ClaudeShift-CLI](https://github.com/Fauzan-Fz/ClaudeShift-CLI)

## 📝 License

MIT — see [LICENSE](LICENSE)
