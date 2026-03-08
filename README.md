<div align="center">

# 🦎 Skales

### Open Source Local AI Agent for Windows & macOS

**Download. Install. Done.**
**No Docker. No Terminal. No cloud. No subscription.**

[![Version](https://img.shields.io/badge/version-5.0.0-1DB954?style=for-the-badge&labelColor=0D1117)](https://skales.app)
[![License](https://img.shields.io/badge/license-BSL_1.1-1DB954?style=for-the-badge&labelColor=0D1117)](https://github.com/skalesapp/skales/blob/main/LICENSE)
[![Platform](https://img.shields.io/badge/Windows_+_macOS-1DB954?style=for-the-badge&labelColor=0D1117&logo=windows&logoColor=white)](https://skales.app)

[**Download**](https://skales.app) · [**Documentation**](https://docs.skales.app) · [**Changelog**](./CHANGELOG.md)

---

</div>

## ⚡ Why Skales?

| | Others | Skales |
|---|---|---|
| **Setup** | Docker, Terminal, CLI | Download EXE/DMG, double-click |
| **RAM** | 1.5–3GB+ | ~300MB |
| **OS** | Linux / Docker required | Windows + macOS native |
| **Time to first agent** | Hours to days | 30 seconds |
| **Your data** | Their servers | Your machine. Period. |

---

## 🚀 Features

**🖥️ Native Desktop App** - Runs as a proper desktop application. System tray, auto-start, graceful shutdown.

**🤖 11+ AI Providers** - OpenRouter, OpenAI, Groq, Anthropic, Google, Mistral, Together AI, xAI, DeepSeek, Cerebras, and local Ollama.

**⚡ ReAct Autopilot** - Autonomous multi-step task execution. Plans, executes, self-corrects. Up to 8 reasoning hops.

**🦁 Lio AI Code Builder** - Multi-AI code generation. Architect designs, Reviewer improves, Builder executes. Build entire projects from plain language.

**🧩 Skill AI** - Ask for a feature Skales doesn't have. It writes the code, saves it as a skill, and it's permanently available. Self-programming AI.

**🧠 Bi-Temporal Memory** - Auto-extracts facts and preferences from conversations. Remembers you across sessions.

**🌐 Browser Control** - Headless Chromium via Playwright. Navigate, click, fill forms, scrape, screenshot any website.

**👁️ Vision** - Desktop screenshot analysis, image recognition, vision-capable model fallback across all channels.

**💬 Telegram & WhatsApp** - Chat with Skales on the go. Full remote admin via Telegram.

**📧 Gmail + 📅 Calendar** - Read, compose, reply, search emails. Create events, get reminders. OAuth integration.

**𝕏 Twitter/X** - Post tweets, read timeline, reply to mentions.

**🛡️ Safety Mode** - Three-level command safety: Safe / Advanced / Unrestricted.

**🤖 AI Group Chat** - Multiple AI personas debate your questions in configurable rounds.

**🛑 Killswitch** - Emergency stop via dashboard, Telegram, or automatic trigger.

**🎨 Image & Video Gen** - Google Imagen 3 and Veo 2 built into the chat.

**🔍 Web Search** - Real-time, cited search results via Tavily.

**🗣️ Voice** - Speak to Skales and hear replies (TTS/STT).

**🔒 Security** - Sandboxed file access, command blacklist, domain blocklist, VirusTotal scanning.

**💾 Backup** - One-click ZIP export/import of all settings, memories, and integrations.

---

## 📦 Installation

| Platform | Download |
|---|---|
| **Windows (x64)** | [Download .exe](https://github.com/skalesapp/skales/releases/latest) |
| **macOS (Apple Silicon M1–M4)** | [Download arm64 .dmg](https://github.com/skalesapp/skales/releases/latest) |
| **macOS (Intel)** | [Download x64 .dmg](https://github.com/skalesapp/skales/releases/latest) |


> Alternatively, download directly from [skales.app](https://skales.app).


```
1. Download for your platform
2. Run the installer (EXE or DMG)
3. Skales opens as a desktop app
4. Add your API key → Start chatting
```

**No Terminal. No Node.js. No Docker. No npm.**

> ⚠️ **First launch:** Windows may show "Windows protected your PC" → Click *More info* → *Run anyway*. macOS: Right-click → *Open* → *Open*. This is because Skales is not code-signed yet. The app runs 100% locally - no data leaves your machine.

---

## 🏗️ Architecture

```
Layer 7  Governance     Safety Mode · Killswitch · Sandbox · Blacklists
Layer 6  Applications   Autopilot · Lio AI · Group Chat · Skill AI · Personas
Layer 5  Memory         Bi-Temporal · Identity · Chat History · JSON Storage
Layer 4  Cognition      ReAct Loop · Planning · Stall Detection
Layer 3  Tools          11+ LLMs · Playwright · File System · Web Search · Vision
Layer 2  Integrations   Telegram · WhatsApp · Gmail · Calendar · Twitter · n8n
Layer 1  Infrastructure 100% Local · No Docker · BYOK · ~300MB RAM
```

---

## 🛡️ Privacy

- **BYOK (Bring Your Own Key)** - API requests go directly from your machine to the provider. No middleman.
- **Local-First** - All data stays in `~/.skales-data` on your machine.
- **Offline Capable** - With Ollama, Skales works entirely offline.
- **Sandboxed** - File operations run in a workspace sandbox.

---

## 🛠️ Tech Stack

- **Frontend:** Next.js 14 + React + Tailwind CSS
- **Desktop:** Electron 28
- **Backend:** Node.js (standalone server)
- **Storage:** JSON-based local files (`~/.skales-data`)
- **Browser Automation:** Playwright
- **Voice:** Groq Whisper (STT) + Google TTS / ElevenLabs

---

## 📖 The Story

Skales started in early 2025 as a bloated SaaS project. I scrapped it all and rebuilt it via "Vibecoding" - the result is a local-first AI agent that feels like a real product, not a developer tool.

I'm **Mario Simic** - 10+ years in Marketing & Design. I got tired of AI agents that require Terminal setups and Docker containers. So I built something my grandmother could use.

---

## 🤝 Contributing

Found a bug? Have a feature idea? PRs and issues are welcome.

1. Fork the repo
2. Create your branch (`git checkout -b feature/my-feature`)
3. Commit (`git commit -m 'Add my feature'`)
4. Push (`git push origin feature/my-feature`)
5. Open a Pull Request

---

## 🔒 License

[**Business Source License 1.1 (BSL)**](./LICENSE)

**Free for personal & educational use.** Use it, learn from it, build with it.

**Commercial use requires a license:** [dev@mariosimic.at](mailto:dev@mariosimic.at)

See [COMMERCIAL-LICENSE.md](./COMMERCIAL-LICENSE.md) for details.

---

<div align="center">

**[skales.app](https://skales.app)** · **[docs.skales.app](https://docs.skales.app)** · **[@skalesapp](https://x.com/skalesapp)**

Built with ❤️ and Vibecoding by **Mario Simic** from Vienna 🇦🇹

*Not an agent. A buddy.* 🦎

</div>
