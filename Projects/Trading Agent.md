# Trading Agent

An AI running three accounts. Ambitious, high-risk — built with guardrails or not at all.

## The three accounts

1. **Stock brokerage** — funded account, **real money**
2. **Crypto majors** (BTC, ETH, main coins) — **paper trading only** until guardrails prove out
3. **Meme coins / brand-new launches** — **paper only**, smallest size, last in line. New coins are full of rug pulls and honeypots.

## Guardrails (non-negotiable)

- Paper-trade first — even the brokerage gets a shakedown before real dollars
- Small position sizes from day one
- Daily loss limit
- Kill switch
- API keys get trading permission **but never withdrawal permission**

## The brain

Decisions locked 10/20–10/21 — **hybrid split** (don't pick one):
- **Local Hermes 3 8B** (Ollama, $0) does the 24/7 scanning/signal-checking
- **Claude API** fires only on real trade decisions — Haiku for routine checks, Sonnet for the big calls (~$1–3/month total)
- Guardrails live in **code**, not in any model's judgment
- Free-tier routers (FreeLLMAPI/OmniRoute) as backup; never send API keys through free tiers (they train on prompts)

## Status

- **Design session DONE 10/20 evening.** Agent named **Warren Buffett**. Broker: **Alpaca**. Real budget: **$100** (paper account set to $100 too). Guardrails: $25 max position / $5 daily loss halt / -$15 total kill switch / PDT protection coded in. Proving period: 2 weeks paper before real dollars.
- Execution layer: **Vibe-Trading** (native Alpaca connector, paper trading, built-in guardrails) — install weekend project. Research: **FinceptTerminal** (.exe, free data, Ollama agents).
- **Billing flag:** Wells Fargo declined the $10.66 Anthropic charge → auto-reload paused. Diego fixing by Thu 10/22. $5 credit live meanwhile.
- Full step-by-step: [[AI Tools Stack Setup]] steps 1–6
- Draft design doc: `~/workspace/goals/hermes-trading-agent-design/files/trading-design-skeleton-DRAFT.md`
