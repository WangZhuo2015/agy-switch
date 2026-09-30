# agy-switch ⚡

> Multi-Account Switcher & Real-Time Quota Dashboard for Google Antigravity CLI (`agy`).

`agy-switch` is a lightweight, zero-external-dependency CLI companion tool for Google Antigravity developers. If you have multiple Google AI Pro or Ultra accounts, `agy-switch` lets you effortlessly inspect real-time quotas, check reset countdowns, swap active tokens with one keystroke, and launch `agy` with seamless flag pass-through.

---

## ✨ Features

- ⚡ **Concurrent Quota Inspection**: Uses isolated temporary execution sessions to query all account quotas in parallel (~4s total).
- 🕒 **Smart Reset Countdown**: Displays remaining time until quota refill (e.g. `(4h 40m)` or `(6d 23h)`).
- 🎯 **Earliest-Reset-First Smart Selection (`auto`)**: Automatically analyzes all saved accounts and prioritizes the one whose weekly quota resets the soonest. Maximizes overall throughput and prevents unused quota from expiring ("use it or lose it").
- 🛡️ **Mega-Quota Session Supervisor (`watch`)**: Makes multiple accounts feel like one single, massive quota account! Automatically detects quota exhaustion (`RESOURCE_EXHAUSTED` / 429), hot-swaps to the next healthy account, resumes your conversation with `agy -c`, and automatically submits `"Continue"` via `-i` so the agent seamlessly finishes any interrupted work without typing a single word.
- 🔥 **7-Day Rolling Reset Warmup (`warmup`)**: Antigravity quota countdowns only start upon making your first request. `warmup` sends ultra-lightweight probes to untouched 100% accounts in parallel, activating their 7-day rolling reset countdowns concurrently so all accounts refill together.
- 🖥️ **Modern Terminal TUI**: High-contrast, clean ASCII dashboard with visual quota progress bars (`[##########]`), highlighting the recommended best account with `★ BEST`.
- ⌨️ **Intuitive Keybindings**:
  - `↑` / `↓` or `k` / `j` to select accounts.
  - `Enter` to switch account and launch `agy`.
  - `b` to instantly pick and launch the optimal account.
  - `w` to run rolling reset warmup on all idle accounts.
  - `1` - `9` numerical quick-keys for instant one-touch launch.
  - `a` to log in and add a new account.
- 🚀 **Flag Pass-Through**: Pass any flags directly to `agy` (e.g. `agy-switch -c --dangerously-skip-permissions`).
- 🔐 **Zero External Dependencies**: Pure Bash + Python standard library (`curses`). Works right out of the box on Linux / macOS.

---

## 📦 Installation

```bash
# Clone the repository
git clone https://github.com/WangZhuo2015/agy-switch.git

# Make executable and link to your local PATH
cd agy-switch
chmod +x agy-switch
mkdir -p ~/.local/bin
ln -sf "$(pwd)/agy-switch" ~/.local/bin/agy-switch
```

Ensure `~/.local/bin` is in your `$PATH`:
```bash
# For bash/zsh:
export PATH="$HOME/.local/bin:$PATH"

# For fish:
fish_add_path ~/.local/bin
```

---

## 🚀 Usage

### 1. Interactive Dashboard (Default)

Simply run:
```bash
agy-switch
```
Or pass your normal `agy` arguments through:
```bash
# Resume your previous chat session
agy-switch -c

# Skip permission prompts
agy-switch --dangerously-skip-permissions

# Combine multiple flags
agy-switch -c --dangerously-skip-permissions
```

### 2. Smart Automation & Failover (New)

```bash
# Auto-pick the best account (earliest weekly reset) and launch agy with optional flags
agy-switch auto -c
agy-switch auto --dangerously-skip-permissions

# Launch in Mega-Quota Supervisor mode (seamless auto-failover on quota exhaustion)
agy-switch watch
agy-switch watch -c --dangerously-skip-permissions

# Kickstart 7-day rolling reset countdown on all 100% untouched accounts
agy-switch warmup
agy-switch warmup myaccount@gmail.com
```

### 3. Fast CLI Commands

```bash
# Check quota across all accounts concurrently
agy-switch usage all

# Check quota of the currently active account
agy-switch usage

# Fast switch to an account by email or keyword
agy-switch alice
agy-switch myaccount@gmail.com

# List all saved accounts
agy-switch list

# Show the currently active account
agy-switch current

# Guided login to add a new account
agy-switch add

# Save the currently active token as a profile
agy-switch save

# Export all account profiles to a portable archive
agy-switch export [my-accounts.tar.gz]

# Import account profiles on a new machine
agy-switch import my-accounts.tar.gz

# Delete a saved profile
agy-switch delete <keyword>
```

---

## 🛠️ How It Works

Antigravity CLI stores OAuth credentials under `~/.gemini/antigravity-cli/antigravity-oauth-token`, and Antigravity Desktop stores them under `~/.gemini/jetski-standalone-oauth-token`.

`agy-switch` automatically discovers and synchronizes tokens from both CLI and Desktop into unified profiles under `~/.gemini/antigravity-cli/profiles/` with zero manual configuration. Whenever you switch accounts or log into a new account, the active credentials are synchronized across both CLI and Desktop simultaneously. Quota inspections query official `/usage` slash command responses concurrently in temporary isolated environments to avoid blocking or interfering with active terminal sessions.

---

## 📄 License

MIT License. Feel free to use, modify, and contribute!
