# Overnight Check — 2026-10-28

**Ran from:** Claude Code cloud container (Linux), not ORTIZ. It can't reach the laptop, so no step touched it.
**Actions taken:** none on any machine. No orders placed, nothing installed, no scheduled tasks enabled or disabled.
**Report location:** `C:\Users\anjel\warren\` isn't reachable from here, so this report is in the vault at `Inbox/overnight-report-2026-10-28.md`.

## 1. Collectors — FAIL (not checked)

No Windows host here, so `Get-ScheduledTask` / `schtasks` couldn't run. The task names below come from the vault build prompts (`Claude Code Collector Prompts.md`). They are **not** confirmed against Task Scheduler.

| Collector | Expected task name | Enabled | Last run | Last result |
|---|---|---|---|---|
| Kalshi 15-min | `Kalshi15M` | unknown | unknown | unknown |
| Kalshi hourly | `KalshiHourly` | unknown | unknown | unknown |
| BTC cycle | `BTCCycle` | unknown | unknown | unknown |
| Power Radar | `PowerRadar` | unknown | unknown | unknown |
| Master of Coin | `MasterOfCoin` | unknown | unknown | unknown |

Note: the vault never logs a "Pn live" for any of these. The Oct 24 grind pack still lists P1/P2/P7 as to-dos. The vault may just be behind.

## 2. Kalshi v2 — FAIL (not checked)

- `kalshi-15m\paper_log_v2.csv`: not reachable. Existence and row count unknown.
- `tracker.log` CAP-SKIP on windows ≥ 80¢: not checked.
- `python summary.py v2`: not run, so there's no output to include.

## 3. Ollama — FAIL (not checked, not started)

- `localhost:11434` (this container): connection refused. Ollama isn't installed here.
- `100.65.199.16:11434` (ORTIZ via Tailscale): timed out after 6 s. This container isn't on the tailnet, so this says nothing about ORTIZ's state.
- Ollama not started, and `hermes3:8b-llama3.1-q8_0` not tested: there was no machine to start it on.
- Bound address: **unknown**. Last recorded in the 10-21 handoff: `0.0.0.0:11434` via machine-level `OLLAMA_HOST`. The "Ollama lockdown" (rebind to the Tailscale IP only) was still on the to-do list as of 10-24.

## To get real numbers

Run the same prompt in a Claude Code session on ORTIZ: either the Claude Desktop app, or `claude remote-control` started in `C:\Users\anjel\warren`, which then shows up in the Claude Code app.
