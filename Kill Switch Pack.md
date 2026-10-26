# Kill Switch Pack — adopted Sat Oct 24, 2026

## The standing rule (Diego confirmed 2026-10-24, ~6:46 PM CT)
If Diego says **"kill switch"** to Everest, Everest freezes everything instantly:
- All watchers / collectors / evaluator crons disabled
- No sends, no purchases, no new jobs
- Freeze holds until Diego says **"resume"**

(Everest controls only its own jobs and connector use. Provider-side revokes below are Diego's taps — Everest can't revoke Diego's accounts for him.)

## If a compromise is ever suspected — do in this order
1. **Tailscale / laptop ORTIZ** (~30 sec): Tailscale admin → remove device **'muse'**, or run `tailscale down` on ORTIZ. Cuts Everest's laptop + Ollama access.
2. **Agent Gmail (everestdca@gmail.com)** — the mail master key: revoke the connector first (Muse Settings → Connectors, or myaccount.google.com/permissions), change the password second, tell Everest third.
3. **Google Calendar** — same Google revoke as #2 covers it (agent account).
4. **Plaid (Wells Fargo + Fidelity)** — disconnect at my.plaid.com. Read-only anyway: it can see balances, it cannot move money.
5. **Secure Vault saved logins** (Discord, TXU, Patreon) — only Diego can delete these, in Muse Settings. Changing the site's password + "log out all devices" also invalidates them.
6. **Anthropic API key** — revoke in the Anthropic console.

## Pack rules that stand at all times
- Alpaca stays **paper only**. Live keys never pass through Everest or any third-party tool — Diego pastes them himself, only into the official app.
- Agent Gmail: unique password + authenticator 2FA. Personal Gmail (darguell22@gmail.com) stays disconnected from Everest by design.
- Retirement money is **never** raided for the Wells Fargo card.
- Third-party skills/tools: SkillSpector scan before install; never route API, wallet, or brokerage credentials through them.

## Resume procedure
Diego says "resume" → Everest re-enables the frozen crons, confirms each one is running again, and reports anything that errored while frozen.
