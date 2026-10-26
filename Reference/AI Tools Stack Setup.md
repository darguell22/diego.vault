# AI Tools Stack — Laptop Setup Guide
Prepared 2026-10-21 for Diego. Windows 11, RTX 5070 Ti 12GB, 32GB RAM.
Do these in order — each layer builds on the last. Total new spend: $0.

---

## Step 0 — Before you start
- [ ] Laptop plugged in, on Wi-Fi
- [ ] ~30GB free disk space (Docker images + models add up)
- [ ] A GitHub account (for cloning repos if needed)

### Security rule (applies to every step below)
Before installing any third-party skill, MCP server, or repo with executable scripts, scan it with NVIDIA's free SkillSpector:
```
git clone https://github.com/NVIDIA/SkillSpector.git
```
Point it at the skill/repo and check the 0–100 risk score. Don't install anything scoring HIGH/CRITICAL. Skills with bundled scripts are 2.12x more likely to be vulnerable — and never paste API keys, wallet keys, or brokerage credentials into a third-party tool.

---

## Step 1 — Docker Desktop (the foundation)
Everything except two apps runs in Docker. This avoids Windows dependency hell.

1. Download Docker Desktop for Windows: https://www.docker.com/products/docker-desktop/
2. During install, keep **WSL 2 backend** checked
3. Reboot when it asks, then launch Docker Desktop and wait until it says "Running"
4. Verify: open PowerShell and run `docker --version` → should print a version number

## Step 2 — Ollama + Hermes 3 8B (the free brain)
This is the shared $0 model every tool below can use.

1. Download Ollama for Windows: https://ollama.com/download
2. Install, then in PowerShell: `ollama pull hermes3:8b-llama3.1-q8_0` (~8.5GB — takes a few minutes)
3. Verify: `ollama run hermes3:8b-llama3.1-q8_0 "Say hi"` → should reply
4. Leave Ollama running — it serves on `http://localhost:11434` (OpenAI-compatible endpoint other tools connect to)

## Step 3 — LibreChat (one chat UI for everything)
1. In PowerShell:
   ```
   git clone https://github.com/danny-avila/LibreChat.git
   cd LibreChat
   docker compose up -d
   ```
2. Open http://localhost:3080, create your account
3. Add endpoints: your Anthropic API key (when billing is fixed), Ollama (`http://host.docker.internal:11434`), any free-tier routers
4. Verify: send a message using the Ollama/Hermes endpoint → should answer with $0 spent

## Step 4 — FinceptTerminal (trading research)
1. Download the Windows x64 `.exe` installer from https://github.com/Fincept-Corporation/FinceptTerminal (Releases)
2. Install and launch — no build needed
3. Connect free data feeds (Yahoo Finance works out of the box)
4. Point its AI agents at Ollama (local = $0)
5. Verify: pull up a stock chart and ask an agent a question about it

## Step 5 — Alpaca paper account (for Warren Buffett)
1. Sign up at https://alpaca.markets → open a **paper trading** account
2. In the dashboard, generate paper API keys (KEY + SECRET)
3. In account settings, reset the paper balance to **$100** so the test matches your real budget
4. Save the keys somewhere safe (password manager) — you'll paste them into Vibe-Trading next

## Step 6 — Vibe-Trading (Warren Buffett's body)
1. In PowerShell:
   ```
   git clone https://github.com/HKUDS/Vibe-Trading.git
   cd Vibe-Trading
   ```
   Follow the README's Docker "zero setup" path
2. Paste your Alpaca **paper** keys into its config — never live keys yet
3. Configure the mandate gate to your guardrails:
   - Max position: $25
   - Daily loss halt: $5
   - Total kill switch: -$15 (only you restart it)
4. Set the LLM brain to your local Ollama endpoint (custom endpoints supported)
5. Verify: run it in paper mode and confirm a test order shows up in your Alpaca paper dashboard
6. **Do not connect real money until the 2-week paper proving period is done and profitable**

## Step 7 — hyperframes (explainer videos)
1. Install Node.js 22+: https://nodejs.org
2. Install FFmpeg: `winget install Gyan.FFmpeg` (or from https://www.gyan.dev/ffmpeg/builds/)
3. In PowerShell:
   ```
   npx hyperframes init explainer-test
   npx hyperframes preview
   ```
4. Edit the HTML to make kinetic captions / data viz, then `npx hyperframes render` → MP4 out
5. Verify: you get a real .mp4 file, $0 spent

## Step 8 — Open-Generative-AI (thumbnails + video test)
1. Download the Windows `.exe` from https://github.com/Anil-matcha/Open-Generative-AI (Releases)
2. Windows SmartScreen will warn (unsigned) → "More info" → "Run anyway"
3. Generate a thumbnail set locally — free, runs on your 5070 Ti
4. For the Aguacate test: set up Wan2GP (separate Python/PyTorch install per its docs) and compare a local clip vs a Kling clip
5. Decision: if local quality is close, the ~$250–400 season cost drops to ~$0

## Step 9 — Open-LLM-VTuber (clips avatar host)
1. Follow the Docker path in https://github.com/Open-LLM-VTuber/Open-LLM-VTuber docs
2. Point its LLM at local Ollama
3. Create a **custom, commercially-safe avatar** — the sample Live2D models are NOT licensed for commercial use
4. Verify: voice conversation works offline
5. Note: v1 is maintenance-only; don't deep-customize its internals

## Step 10 — camofox (research scraping for the Shopify team)
1. `npx @askjo/camofox-browser` (server starts on :9377), or Docker via `.\build.ps1 up`
2. First run downloads ~300MB — that's normal
3. Test: scrape competitor prices for the 3 shortlisted products (acne patches, magnetic lashes, tumblers)
4. Rules: respect sites' ToS, don't hammer endpoints

## Step 11 — Claude Code (agentic coding on your laptop)
You have Claude Pro ($20/mo, confirmed 2026-10-22), so this is included — no extra cost. This is the real Claude Code: a terminal agent that edits files and runs commands on your laptop directly, instead of you copy-pasting between the Claude app and PowerShell. This is what makes the Warren build steps below dramatically easier.
1. Install Node.js 22+ LTS: https://nodejs.org (you'll need this for hyperframes later anyway)
2. In PowerShell, exactly this: `npm install -g @anthropic-ai/claude-code`
3. Run `claude` → it opens a browser login → sign in with your Claude Pro account
4. Verify: `claude --version` → should print a version number
5. Usage: `cd` into a project folder, run `claude`, and tell it what to do in plain words. It draws from your Pro quota (5-hour + weekly pool, shared with the Claude app).
6. Your one-time "Reset for free" card (Settings → Usage → Resets on claude.ai): EXPIRES tonight 11:59 PM PT. Don't burn it early — save it for when you actually hit a limit, but use it or lose it tonight.
7. Caution: adding MCP servers to Claude Code falls under your security rule — scan with SkillSpector first. REA (the viral reverse-engineering repo) is a skip: IP-sketchy lane, not yours.

## Step 12 — Strawberry (agentic browser for outreach research)
No-code AI browser — its companions click/type in tabs to do research tasks for you. Free plan: 2,000 AI credits/month.
1. Download for Windows: https://strawberrybrowser.com
2. Create a companion for Proper Papers lead sourcing (prompt in your vault note — ask Everest for the paste-ready version for your current job)
3. Rules: public-web research only — don't log it into your bank, brokerage, or anything with money. Don't let it auto-send emails/DMs; it drafts, you approve and send.
4. Verify: run a 5-shop test batch and check its results against Google before scaling up

---

## Install order cheat sheet
| Order | What | Time |
|---|---|---|
| 1 | Docker Desktop | ~15 min + reboot |
| 2 | Ollama + Hermes 3 8B | ~10 min (download) |
| 3 | LibreChat | ~10 min |
| 4 | FinceptTerminal | ~5 min |
| 5 | Alpaca paper account + keys | ~10 min |
| 6 | Vibe-Trading | ~20 min |
| 7–10 | hyperframes, Open-Generative-AI, VTuber, camofox | as needed |
| 11 | Claude Code | ~10 min, needs Node.js |
| 12 | Strawberry | ~10 min, anytime |

Start with 1–4 tonight; that's a working research station. Vibe-Trading (6) is the weekend project.
