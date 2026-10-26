# Claude handoff: Warren Buffett Trading Agent
_Paste this whole note into Claude, then ask your question._

## Who
Diego, Austin TX. Building an automated stock-trading agent. Money is tight — free/local-first, quote any cost before spending.

## The agent
- **Name:** Warren Buffett. **Broker:** Alpaca. **Real budget:** $100 (paper account also set to $100).
- **Three tracks:** (1) stocks — real money after proving; (2) crypto majors (BTC/ETH) — paper only; (3) meme coins — paper only, smallest size, last.
- **Guardrails (non-negotiable, live in code):** $25 max position · $5 daily loss = halt for the day · -$15 total drawdown = full stop, only Diego restarts · PDT protection (max 3 day-trades/5 days) · API keys never get withdrawal permission.
- **Brain (hybrid):** local Hermes 3 8B via Ollama does 24/7 scanning (free); Claude API fires only on real trade decisions (Haiku routine, Sonnet big calls, ~$1–3/mo). Never send keys through free-tier routers.
- **Execution layer:** Vibe-Trading (Docker, native Alpaca connector, paper mode, mandate-gated orders, kill switch, audit ledger). Research: FinceptTerminal (free data, Ollama agents).
- **Billing flag:** Anthropic auto-reload is paused (declined charge); $5 credit live. Diego fixing by Thu 10/22.

## Current state (2026-10-21, evening — live build in progress)
- ✅ **Local brain LIVE:** Ollama installed on ORTIZ (Windows 11 laptop); `hermes3:8b-llama3.1-q8_0` (8.5GB, Q8) pulled and verified responding. Also present: `qwen3:14b` (9.3GB, pre-existing).
- ✅ **Everest tunnel:** Tailscale joined from Everest VM (device `muse`); laptop `ortiz` at 100.65.199.16. OLLAMA_HOST=0.0.0.0 (machine env) + Windows Firewall inbound rule for 11434. Gotcha hit: a stale ollama.exe survived tray-quits and kept binding 127.0.0.1 — fixed with `taskkill /F /IM ollama.exe` + fresh launch; now on 0.0.0.0:11434. Verified end-to-end: Everest queried /api/tags and got a test generation back through the tunnel ("Warren is online").
- ⚠️ Naming trap: "Hermes Agent" / "Hermes Desktop" (Nous Research apps) are NOT the model — Diego installed Hermes Agent by mistake, uninstalled. The brain is the Hermes 3 model inside Ollama.
- ⬜ Next: FinceptTerminal install → Docker Desktop → Vibe-Trading → Alpaca paper keys → 2-week paper proving period.
- ⬜ Still pending: Alpaca account signup + paper API keys (phone-doable); Anthropic billing fix (auto-reload still paused, $5 credit live).

## Ask Claude for
Whatever you need: e.g. "write the Vibe-Trading mandate config for these guardrails," "review this strategy for the $100 account," "draft the paper-trading journal template."
