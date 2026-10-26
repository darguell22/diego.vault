# 12-Factor Agents — Build Standard

Source: humanlayer/12-factor-agents (Dex Horthy, HumanLayer) — "Principles for building reliable LLM applications." The agent-world equivalent of the 12-Factor App methodology. Free on GitHub. Verified real 2026-10-22 (via @siroccomask TikTok).

**The 12 factors:**
1. **Natural language to tool calls** — the agent's job is turning intent into actions.
2. **Own your prompts** — prompts are code; version and control them (this is what the Claude handoff notes do).
3. **Own your context window** — manage what the agent sees; fresh scoped context per job beats one endless chat.
4. **Tools are just structured outputs** — keep tool interfaces clean and typed.
5. **Unify execution state and business state** — one source of truth (this is what the Warren dashboard is).
6. **Launch / pause / resume with simple APIs** — agents should be stoppable and resumable.
7. **Contact humans with tool calls** — approvals and check-ins are first-class, not afterthoughts.
8. **Own your control flow** — don't hand the loop to the model; the harness decides.
9. **Compact errors into context window** — failures come back as concise signals, not raw dumps.
10. **Small, focused agents** — many narrow agents beat one bloated mega-agent (the 502 watcher / Kalshi tracker / brain check pattern).
11. **Trigger from anywhere, meet users where they are** — cron, chat, dashboard; the agent doesn't care where the trigger came from.
12. **Make your agent a stateless reducer** — state lives outside the agent (files, dashboard), so any run can pick up where the last left off.

**How this applies to Diego's agent teams (Shopify + viral clips):**
- One small agent per job (researcher, sourcer, lister, poster) — factor 10.
- Prompts versioned in the vault / Claude handoff notes — factor 2.
- Fresh handoff note per run, not endless context — factor 3.
- Dashboard as the single source of truth — factor 5.
- Nothing outward-facing (posts, listings, purchases) without Diego's approval — factor 7.
- State in files and the dashboard so runs survive restarts — factor 12.
