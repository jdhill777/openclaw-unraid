# OpenClaw for Unraid

[![Unraid](https://img.shields.io/badge/Unraid-CA%20Template-orange)](https://unraid.net/)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-v2026.1.29-blue)](https://github.com/openclaw/openclaw)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow?logo=buymeacoffee)](https://buymeacoffee.com/jdhill777)

Community Applications template for [OpenClaw](https://github.com/openclaw/openclaw) (formerly ClawdBot/MoltBot) — a powerful, self-hosted AI assistant that runs locally on your Unraid server.

![OpenClaw Dashboard](screenshot.png)

## 🦞 What is OpenClaw?

OpenClaw is a personal AI assistant you run on your own devices. It answers you on the channels you already use and keeps your data completely local.

### Multi-Channel Messaging
Connect to all your favorite platforms from one assistant:
- 💬 **WhatsApp** — via WhatsApp Web/Baileys
- ✈️ **Telegram** — Bot API with DMs + groups
- 🎮 **Discord** — Full bot integration with guilds
- 💼 **Slack** — Workspace integration via Bolt
- 📧 **Google Chat** — Workspace chat support
- 📱 **Signal** — Secure messaging
- 🍎 **iMessage** — macOS integration
- 🔷 **Microsoft Teams** — Enterprise chat
- 🌐 **Matrix**, **Mattermost**, **BlueBubbles**, and more via plugins

### Powerful Features
- 🧠 **Multi-Agent Routing** — Route different channels/users to isolated agents with separate workspaces
- 📁 **File Management** — Read, write, and organize files on your server
- ⚡ **Shell Commands** — Execute scripts, manage Docker, automate anything
- 🌐 **Browser Control** — Research, fetch data, interact with web pages
- ⏰ **Cron Jobs** — Schedule tasks, reminders, and automated workflows
- 🧩 **Skills System** — Extend capabilities with bundled or custom skills
- 🎤 **Voice Wake + Talk Mode** — Always-on speech with ElevenLabs TTS
- 📺 **Live Canvas** — Agent-driven visual workspace
- 📱 **Mobile Nodes** — iOS and Android companion apps
- 🖥️ **macOS Menu Bar App** — Quick access companion

### Your Data, Your Server
Your files, workspace, and configuration stay **100% on your Unraid server**. Conversations are processed through your chosen LLM provider's API (Anthropic, OpenAI, etc.) but nothing is stored on third-party servers beyond their standard API processing. For fully local operation, you can use [Ollama](https://ollama.ai) with local models.

## 📋 Requirements

- Unraid 6.x or 7.x
- Docker support enabled
- API key from one of:
  - [Anthropic](https://console.anthropic.com) (Claude) — **recommended**
  - [OpenRouter](https://openrouter.ai) (100+ models)
  - [OpenAI](https://platform.openai.com) (GPT models)
  - [Google AI Studio](https://aistudio.google.com) (Gemini)
  - [Groq](https://console.groq.com) (Fast Llama/Mixtral)

### Getting an Anthropic API Key (Recommended)

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Sign up or log in
3. Add a payment method (Settings → Billing)
4. Go to **API Keys** and create a new key
5. Copy the key (starts with `sk-ant-`)

**Pricing:** Pay-as-you-go, starting at $5 credit. Most personal usage costs $5-20/month depending on how much you chat.

> **Note:** This is separate from a Claude Pro subscription ($20/mo). Claude Pro is for the claude.ai web app — API access requires credits from the console. If you have Claude Pro or Max, see below for subscription-based auth.

### Using Claude Pro/Max Subscription (Alternative)

Already have a Claude Pro ($20/mo) or Max subscription? You can get a subscription-based token from Claude Code instead of buying API credits.

**On your Mac or PC (not Unraid):**

1. Make sure you have [Claude Code](https://claude.ai/code) installed and logged in with your Pro/Max account
2. Run this command:
   ```bash
   claude setup-token
   ```
3. Follow the prompts — it will output a token (starts with `sk-ant-oat01-...`)
4. Copy that token

**On Unraid:**

5. In the OpenClaw template, set the environment variable:
   - **Name:** `CLAUDE_CODE_OAUTH_TOKEN`
   - **Value:** (paste the token from step 3)

That's it! OpenClaw will use your subscription instead of API credits.

> **Tip:** The token expires periodically. If you get auth errors, run `claude setup-token` again to get a fresh token.

### Using Non-Anthropic Providers (OpenAI, Gemini, Groq, OpenRouter)

OpenClaw defaults to Anthropic's Claude model. **If you're using a different provider, you must change the default model after installation:**

1. Install OpenClaw with your API key (e.g., `GEMINI_API_KEY`)
2. Open the Control UI
3. Go to **Config** tab
4. Find `agents.defaults.model.primary` and change it to match your provider:

| Provider | Model Example |
|----------|---------------|
| Anthropic | `anthropic/claude-sonnet-4-5` (default) |
| Google Gemini | `google/gemini-2.0-flash` |
| OpenAI | `openai/gpt-4o` |
| Groq | `groq/llama-3.1-70b-versatile` |
| OpenRouter | `openrouter/anthropic/claude-3-sonnet` |

5. Save the config and restart the container

> **Why is this needed?** OpenClaw doesn't auto-detect which provider you're using from the API key. The default model is Anthropic — if you set a Gemini key but leave the default model, you'll get "No API key found for anthropic" errors.

## 🚀 Quick Start

### Step 1: Install from Community Apps

1. Search for **OpenClaw** in Community Applications
2. Click **Install**
3. Fill in the form:
   - **Gateway Token**: Recommend generating with `openssl rand -hex 24`, but can be any value
   - **LLM API Key**: Your Anthropic, OpenRouter, or other provider key
4. Click **Apply**

That's it! The template automatically configures everything.

### Step 2: Access the Control UI

Open your browser to:

```
http://YOUR-UNRAID-IP:18789/?token=YOUR_GATEWAY_TOKEN
```

**Important:** You must append `?token=YOUR_TOKEN` to the URL for authentication.

Example: `http://192.168.1.41:18789/?token=mySecretToken123`

## ⚙️ Configuration

### Template Settings Reference

This table documents all settings available in the Unraid template:

| Setting | Type | Required | Default | Description |
|---------|------|----------|---------|-------------|
| **Ports** |
| Control UI Port | Port | ✅ Yes | `18789` | Web UI and Gateway API port. Access at `http://IP:18789/?token=TOKEN` |
| **Paths** |
| Config Path | Path | ✅ Yes | `/mnt/user/appdata/openclaw/config` | OpenClaw configuration, sessions, and credentials |
| Workspace Path | Path | ✅ Yes | `/mnt/user/appdata/openclaw/workspace` | Agent workspace for files, memory, and projects |
| Projects Path | Path | ❌ Optional | `/mnt/user/appdata/openclaw/projects` | Additional path for coding projects (advanced) |
| Homebrew Path | Path | ❌ Optional | `/mnt/user/appdata/openclaw/homebrew` | Persistent Homebrew packages. Enables skills requiring brew/go/npm. Auto-installs on first start (~60 sec). |
| **Authentication** |
| Gateway Token | Variable | ✅ Yes | — | Secret token for API/UI access. Generate: `openssl rand -hex 24` |
| **LLM Providers** (at least one required) |
| Anthropic API Key | Variable | ⚡ Recommended | — | Claude models. Get from [console.anthropic.com](https://console.anthropic.com) |
| OpenRouter API Key | Variable | ❌ Optional | — | 100+ models via single API. Get from [openrouter.ai](https://openrouter.ai) |
| OpenAI API Key | Variable | ❌ Optional | — | GPT models. Get from [platform.openai.com](https://platform.openai.com) |
| Gemini API Key | Variable | ❌ Optional | — | Google Gemini. Get from [aistudio.google.com](https://aistudio.google.com) |
| Groq API Key | Variable | ❌ Optional | — | Fast Llama/Mixtral. Get from [console.groq.com](https://console.groq.com) |
| **Messaging Channels** (configure after install) |
| Discord Bot Token | Variable | ❌ Optional | — | Discord bot integration. Can also configure via Control UI |
| Telegram Bot Token | Variable | ❌ Optional | — | Telegram bot from [@BotFather](https://t.me/BotFather). Can also configure via Control UI |
| **Advanced** |
| Gateway Port | Variable | ❌ Optional | `18789` | Override if port 18789 is in use |
| Web Search API Key | Variable | ❌ Optional | — | Brave Search API for web search (2000 free/month at [brave.com/search/api](https://brave.com/search/api)) |

### Volume Mounts

| Container Path | Host Path | Description |
|----------------|-----------|-------------|
| `/root/.openclaw` | `/mnt/user/appdata/openclaw/config` | Config file, sessions, credentials, WhatsApp auth |
| `/home/node/clawd` | `/mnt/user/appdata/openclaw/workspace` | Agent workspace (files, memory, AGENTS.md, etc.) |
| `/projects` | `/mnt/user/appdata/openclaw/projects` | Optional coding projects folder |
| `/home/linuxbrew/.linuxbrew` | `/mnt/user/appdata/openclaw/homebrew` | Homebrew packages (persists brew/go/npm installs) |

### Config File Reference (`openclaw.json`)

The main configuration file is stored at:
```
/mnt/user/appdata/openclaw/config/openclaw.json
```

Key configuration sections (see [full docs](https://docs.openclaw.ai/gateway/configuration)):

| Section | Purpose | Example |
|---------|---------|---------|
| `gateway` | Core gateway settings | `{ "bind": "lan", "port": 18789 }` |
| `gateway.auth` | Authentication mode | `{ "mode": "token" }` |
| `gateway.controlUi` | Web UI settings | `{ "allowInsecureAuth": true }` |
| `channels` | Messaging platform configs | Discord, Telegram, WhatsApp, Slack, etc. |
| `channels.discord` | Discord bot settings | `{ "enabled": true, "token": "..." }` |
| `channels.telegram` | Telegram bot settings | `{ "enabled": true, "botToken": "..." }` |
| `channels.whatsapp` | WhatsApp Web settings | `{ "allowFrom": ["+1234567890"] }` |
| `agents` | Agent configuration | Model selection, workspace, sandbox |
| `agents.defaults.model` | Default LLM model | `{ "primary": "anthropic/claude-sonnet-4-5" }` |
| `agents.defaults.workspace` | Agent workspace path | `"~/.openclaw/workspace"` |
| `tools` | Tool permissions | Web search, browser, elevated access |
| `cron` | Scheduled jobs | Reminders, automations, periodic tasks |
| `messages` | Message handling | Prefixes, reactions, queue behavior |

**Minimal config (auto-created on first run):**
```json
{
  "gateway": {
    "mode": "local",
    "bind": "lan",
    "controlUi": { "allowInsecureAuth": true },
    "auth": { "mode": "token" }
  }
}
```

**Example with Discord + custom model:**
```json
{
  "gateway": {
    "mode": "local",
    "bind": "lan",
    "controlUi": { "allowInsecureAuth": true },
    "auth": { "mode": "token" }
  },
  "channels": {
    "discord": {
      "enabled": true,
      "token": "YOUR_DISCORD_BOT_TOKEN",
      "dm": { "enabled": true, "policy": "allowlist", "allowFrom": ["YOUR_DISCORD_USER_ID"] },
      "guilds": {
        "YOUR_GUILD_ID": {
          "channels": { "general": { "allow": true } }
        }
      }
    }
  },
  "agents": {
    "defaults": {
      "model": { "primary": "anthropic/claude-sonnet-4-5" }
    }
  }
}
```

📚 **Full configuration reference:** [docs.openclaw.ai/gateway/configuration](https://docs.openclaw.ai/gateway/configuration)

### Connecting Messaging Channels

After installation, configure channels via the Control UI **Config** page, or edit directly:

```
/mnt/user/appdata/openclaw/config/openclaw.json
```

Example adding Discord + Telegram:
```json
{
  "gateway": { ... },
  "channels": {
    "discord": {
      "enabled": true,
      "token": "YOUR_DISCORD_BOT_TOKEN"
    },
    "telegram": {
      "enabled": true,
      "botToken": "YOUR_TELEGRAM_BOT_TOKEN"
    }
  }
}
```

📚 Full channel setup guides: [OpenClaw Docs - Channels](https://docs.openclaw.ai/channels)

## 🔄 Updating

**Via Unraid Docker UI:**
1. Docker tab → Click OpenClaw icon → Check for Updates
2. Apply update

**Via command line:**
```bash
docker pull ghcr.io/openclaw/openclaw:latest
docker restart OpenClaw
```

## 🔧 Troubleshooting

### "disconnected (1008): control ui requires HTTPS or localhost"

Ensure you're accessing with your token:
```
http://YOUR-IP:18789/?token=YOUR_TOKEN
```

If the error persists, verify the config file exists and contains `allowInsecureAuth`:
```bash
cat /mnt/user/appdata/openclaw/config/openclaw.json
```

### Container won't start / "Missing config" error

Manually create the config:
```bash
mkdir -p /mnt/user/appdata/openclaw/config
echo '{"gateway":{"mode":"local","bind":"lan","controlUi":{"allowInsecureAuth":true},"auth":{"mode":"token"}}}' > /mnt/user/appdata/openclaw/config/openclaw.json
docker restart OpenClaw
```

### Check logs for errors
```bash
docker logs OpenClaw 2>&1 | tail -50
```

## 🖥️ Install Before Community Apps Approval

Not in CA yet? No problem — here's how to get running now:

### Option 1: Download Template File (Unraid 6.10+)

Template Repositories were removed in Unraid 6.10. Use this method instead:

1. Download [openclaw.xml](https://raw.githubusercontent.com/jdhill777/openclaw-unraid/master/openclaw.xml)
2. Copy it to your Unraid flash drive:
   ```
   /boot/config/plugins/dockerMan/templates-user/openclaw.xml
   ```
3. Go to **Docker** tab → **Add Container**
4. Select **OpenClaw** from the Template dropdown
5. Fill in your settings:
   - **Gateway Token**: Any secret value (or generate with `openssl rand -hex 24`)
   - **Anthropic API Key**: Get from [console.anthropic.com](https://console.anthropic.com) (recommended)
   - Or use OpenRouter/OpenAI/Gemini/Groq instead
6. Click **Apply**

### Option 2: Add Template Repository (Unraid 6.9 and earlier)

If you're on Unraid 6.9.x or earlier, you can add the template repo directly:

1. Go to **Docker** tab in Unraid
2. Click **Add Container**
3. At the bottom, click **Template repositories**
4. Add this URL: `https://github.com/jdhill777/openclaw-unraid`
5. Click **Save**
6. Now select **OpenClaw** from the Template dropdown
7. Fill in your settings and click **Apply**

### Option 3: Manual Docker Run

For those who prefer the command line:

```bash
# Create directories
mkdir -p /mnt/user/appdata/openclaw/config
mkdir -p /mnt/user/appdata/openclaw/workspace
mkdir -p /mnt/user/appdata/openclaw/homebrew

# Run container (replace YOUR_TOKEN and YOUR_API_KEY)
# Config and Homebrew are set up automatically on first run
docker run -d \
  --name OpenClaw \
  --network bridge \
  --user root \
  --hostname OpenClaw \
  --restart unless-stopped \
  -p 18789:18789 \
  -v /mnt/user/appdata/openclaw/config:/root/.openclaw:rw \
  -v /mnt/user/appdata/openclaw/workspace:/home/node/clawd:rw \
  -v /mnt/user/appdata/openclaw/homebrew:/home/linuxbrew/.linuxbrew:rw \
  -e OPENCLAW_GATEWAY_TOKEN=YOUR_TOKEN \
  -e ANTHROPIC_API_KEY=sk-ant-YOUR_KEY \
  ghcr.io/openclaw/openclaw:latest \
  sh -c "mkdir -p /root/.openclaw /home/linuxbrew; [ -f /root/.openclaw/openclaw.json ] || echo '{\"gateway\":{\"mode\":\"local\",\"bind\":\"lan\",\"controlUi\":{\"allowInsecureAuth\":true},\"auth\":{\"mode\":\"token\"}}}' > /root/.openclaw/openclaw.json; [ -x /home/linuxbrew/.linuxbrew/bin/brew ] || { echo Installing Homebrew... && NONINTERACTIVE=1 curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh | bash; }; eval \$(/home/linuxbrew/.linuxbrew/bin/brew shellenv) 2>/dev/null || true; exec node dist/index.js gateway --bind lan"
```

> **Note:** First start will take ~60 seconds to install Homebrew. Subsequent starts are fast since Homebrew persists in the volume.

**Required tokens:**
- `OPENCLAW_GATEWAY_TOKEN` — Any secret value you choose. You'll need this to access the Control UI.
- At least one LLM API key:
  - `ANTHROPIC_API_KEY` — **Recommended.** Get from [console.anthropic.com](https://console.anthropic.com)
  - `OPENROUTER_API_KEY` — Access 100+ models. Get from [openrouter.ai](https://openrouter.ai)
  - `OPENAI_API_KEY` — GPT models. Get from [platform.openai.com](https://platform.openai.com)
  - `GEMINI_API_KEY` — Google Gemini. Get from [aistudio.google.com](https://aistudio.google.com)
  - `GROQ_API_KEY` — Fast Llama/Mixtral. Get from [console.groq.com](https://console.groq.com)

## 📚 Resources

- **Unraid Support Thread:** https://forums.unraid.net/topic/196865-support-openclaw-ai-personal-assistant/
- **OpenClaw Website:** https://openclaw.ai
- **Documentation:** https://docs.openclaw.ai
- **GitHub:** https://github.com/openclaw/openclaw
- **Discord Community:** https://discord.gg/clawd
- **Getting Started Guide:** https://docs.openclaw.ai/start/getting-started

## 📝 License

This template is released under the [MIT License](LICENSE).

OpenClaw is open-source under the MIT License — see the [OpenClaw repository](https://github.com/openclaw/openclaw).

## ☕ Support the Maintainer

If this template saved you time, consider buying me a coffee!

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow?logo=buymeacoffee&style=for-the-badge)](https://buymeacoffee.com/jdhill777)

*Note: I maintain the Unraid template — for the OpenClaw project itself, see their [GitHub Sponsors](https://github.com/sponsors/openclaw).*

## 🙏 Credits

- **OpenClaw Team** — Peter Steinberger ([@steipete](https://twitter.com/steipete)) and contributors
- **Template Author** — [@jdhill777](https://github.com/jdhill777)
- **Tested on** — Unraid 7.x

---

🦞 *"EXFOLIATE! EXFOLIATE!" — A space lobster, probably*

**Questions?** Open an issue or join the [OpenClaw Discord](https://discord.gg/clawd).
