# 🔥 6-Hour Grind — Master Prompt Pack (Sat Oct 24)

Everything in run order. Each Claude Code prompt is in its own block — copy exactly, one at a time, wait for it to finish before pasting the next. Total ≈ 6 hours with buffer.

**The switch-off rule (important):** Prompts 3, 6, and 7 in the Collector pack are *migrations*. The moment one of them is running locally on ORTIZ, message Everest **"P7 live"** (or P3 / P6) so I switch OFF my copy the same night — otherwise we double-log.

---

## BLOCK 0 — Pre-flight, by hand (~20 min)

### 1. Agent Gmail (unblocks email, calendar, summaries, this vault's daily zip)
1. Create a brand-new Gmail — this is the *agent* box, not your real life. Suggested name: `d.arguelles.agent@gmail.com` (or similar — your pick).
2. Fresh password you've never used anywhere. Turn on 2-Step Verification with an **authenticator app** (not SMS).
3. Set the recovery email to your personal Gmail.
4. Connect it here: https://agent.meta.ai/connectors/gmail/accounts/link
5. Then reconnect Google Calendar the same way.
6. Message Everest: **"agent gmail connected"** — I'll verify it, reroute the vault batch + summaries to the new box, and email you this pack there as the first test.

Rules for the agent box (already agreed): no bank or personal password resets on it, no forwarding from your real inbox, ever.

### 2. Wake ORTIZ's brain (fixes Oracle)
- Laptop on, Tailscale connected, Ollama running. Quick check in PowerShell:
```
curl http://100.65.199.16:11434/api/tags
```
If that returns your model list, we're good — Oracle's ideator has missed 3 straight runs and has 13+ trends queued waiting for Hermes.

---

## BLOCK 1 — Security (~30 min)

### 3. Kill switch pack — read & confirm (no paste, just say "confirmed" to Everest)
- **"KILL SWITCH"** from you = Everest freezes everything instantly: all watchers/collectors/evaluator disabled, no sends, no purchases, no new jobs, until you say "resume."
- Alpaca stays **paper only**. Live keys never pass through Everest or any third-party tool — you paste them yourself, only ever into the official app.
- Agent Gmail: unique password + authenticator 2FA; if a compromise is ever suspected: revoke the connector first, change the password second, tell Everest third.
- Card rule stands: retirement money is never raided for the Wells Fargo card.

### 4. Ollama lockdown — paste into Claude Code
```
Lock down Ollama on this Windows 11 machine. Right now it listens on 0.0.0.0:11434, which exposes it to the whole local network. I only want it reachable over Tailscale.

1. Show me the current OLLAMA_HOST setting and confirm Ollama is running.
2. Change OLLAMA_HOST to 100.65.199.16:11434 (this machine's Tailscale IP) as a persistent user environment variable so it survives reboots.
3. Restart Ollama so the new binding takes effect.
4. Verify all three: (a) curl http://100.65.199.16:11434/api/tags returns the model list; (b) curl to this machine's LAN IP on port 11434 FAILS; (c) `ollama list` still shows hermes3:8b-llama3.1-q8_0 and qwen3:14b.
5. If anything breaks, revert OLLAMA_HOST to 0.0.0.0:11434 and tell me.
6. At the end, print the exact steps to undo this lockdown.

Do not change any other network or firewall settings.
```

---

## BLOCK 2 — Collectors migration (~90 min)
Open the vault note **`Claude Code Collector Prompts.md`** and run in this order:

1. **Prompt 2** — Master of Coin local runner (seeded with the final Everest state: 10 open / 37 closed / net −28,403¢). Creates the data first.
2. **Prompt 1** — Hourly Kalshi KXBTCD collector.
3. **Prompt 7** — Power Radar local clone (includes the wallet-age profiling). ⚠️ Then message Everest **"P7 live"** — my 30-min Everest sweep switches off.
4. **Prompt 4** — Local Warren Command Dashboard (last — it reads the other collectors' outputs; Everest-only feeds render as dark bays, no fake data).

If gas remains: **Prompt 3** (15-min Kalshi tracker clone — ⚠️ message Everest "P3 live" so my copy switches off, or we double-log) and **Prompt 6** (BTC cycle clone — same rule, "P6 live").

**NEW (added this morning):** the Collector pack now opens with a **Paper → Real Graduation Protocol** — Axiom is the chosen meme execution platform, with a hard gate (paper net positive, ≥30 closed trades, ≥14 days, your explicit "graduate") and Stage 1 manual-real caps ($100 bankroll, $5/trade, −$10/day halt, −$25 kill switch). After Prompt 2, also paste the **Prompt 2 addendum** (graduation_check.py + signals list) — it builds the machinery only; it cannot place orders or hold keys.

---

## BLOCK 3 — LLM Gateway (~25 min tonight; the big build is a Sunday option)

**My recommendation for tonight: LiteLLM.** It gets Warren a working router in ~20 minutes. The custom build is the resume/learning play — its full Saturday prompt is in `weekend-llm-gateway-build/files/build-plan.md` if you'd rather build. Don't do both tonight.

### LiteLLM — paste into Claude Code
```
Install LiteLLM as my local LLM gateway on this Windows 11 machine using Docker Desktop.

Requirements:
1. Pin the image to litellm version 1.83.7 or newer (never `latest` unpinned — there was a supply-chain CVE, CVE-2026-42271, in older versions). Tell me the exact version you're about to use before pulling.
2. Config (config.yaml): two models — "local" routes to Ollama at http://host.docker.internal:11434 with model ollama/hermes3:8b-llama3.1-q8_0; "claude" routes to Anthropic with the key read from the ANTHROPIC_API_KEY environment variable. Set fallbacks: if "local" errors or times out over 20s, fall back to "claude".
3. Run it on port 4000. The API key for the gateway itself: generate a simple local master key and put it in a .env file; .env goes in .gitignore from the first commit. Never hardcode any key.
4. Verify: curl the OpenAI-compatible endpoint at http://localhost:4000/v1/chat/completions with model "local" and a test prompt, show me the response. Then stop Ollama briefly and show the same request succeeding via the Claude fallback (if my API key is set — if not, stop and ask me to set it).
5. Print run/stop commands and how to make it start with Docker Desktop.

Explain each step in one short sentence before doing it.
```

---

## BLOCK 4 — Warren (~60–90 min)
Open the vault note **`Claude Code Kickoff Prompt.md`**:

1. **Warm-up (5 min, by hand):** FinceptTerminal — install from GitHub Releases if you haven't, launch, connect the free Yahoo Finance feed, point its AI at Ollama (`http://localhost:11434`, `hermes3:8b-llama3.1-q8_0`).
2. **Paste the Warren kickoff prompt** (Vibe-Trading via Docker + Alpaca **paper** + guardrails: $25 max position, −$5 daily halt, −$15 kill switch, PDT protection). When it asks for the Alpaca paper keys — you paste them yourself, they never go through Everest or chat.
3. Done = it shows the paper account with the $100 balance. The 2-week proving clock starts tonight.

---

## BLOCK 5 — If gas remains (~60 min)

0. **Axiom account (10 min, free, phone or laptop):** create your account at axiom.trade and **attach a referral code at sign-up — the fee discount can't be added later.** Create the built-in self-custody wallet. Do NOT fund it — funding waits until a strategy passes the graduation gate.
1. **Course-Eater** — vault note `Claude Code Course Eater Prompt.md`: builds the local pipeline (faster-whisper GPU transcription + per-lesson skill extraction), stops after one sample lesson for your review. Any course files it touches get a SkillSpector scan first — standing rule.
2. **Qwen3-TTS voices** — Job 2 in `Claude Code Kickoff Prompt.md` (Aguacate character voices, local + free).
3. **Strawberry** — install Strawberry browser, then vault note `Projects/Proper Papers/Strawberry Outreach Prompts.md`, **Prompt 1** (30 new shops: 5 each from Arlington, Plano, Laredo, Amarillo, Waco, College Station). Never connect Strawberry to anything holding money.

---

## 📱 QUICK HITS — phone, anytime, no laptop needed

**Call TODAY (Sat) 10 AM–12 PM only:** Payless Automotive **(512) 670-3134** — "Do you install customer-supplied spark plugs, and can you confirm the plug part number by VIN?" Miss the window → Dave's Ultimate Automotive (512) 601-5447, Mon–Fri 7:30–6. ⚠️ Left front tire still has the sidewall bubble — easy on the highway until it's handled.

**Text to William Turner (Round Rock Hyundai):**
> Hi William — approving the oil pan reseal under warranty. On the spark plugs: does the $225 diagnostic fee come off the $546.57 quote if you do the plugs, or is it on top? Can you send me the out-the-door total for plugs + the warranty oil pan only, nothing else? Thanks — Diego

**Text to Dad:**
> Hey Dad — quick update on the car. The dealer's real spark plug quote is $546.57 (higher than the ~$350 we talked about). The oil pan leak is covered free under warranty. I found independent shops that can do the plugs for $160–320 total, which would mean fewer $100/month payments back to you. I won't charge anything to your card until I give you the final number first.

**FinWise claim — deadline Thu Oct 29:** search YOUR mail/texts for "FinWise" or "American First Finance." If you find a notice, the claim needs the ID + PIN from it (reissue line: 1-877-419-3877). No notice found = you likely weren't in the breach population.

**Money confirmations Everest still needs from you:**
- Did the **$300 Wells Fargo card payment** go through from the 10/22 check? ($72 minimum is due Nov 4.)
- All In's 3 paper plays from 10/22 — still unsettled in my log. Any results you saw?

**Mushrooms (2 min):** order pink oyster liquid culture — Midwest Grow Kits, $12.50, in stock last check. Liquid culture, not spores.

---

*Prepared by Everest, Sat Oct 24 ~7:55 AM. Long prompts live in the vault notes named above; the new ones (Ollama lockdown, LiteLLM) are inline here. This pack is also in the vault, so it rides the next vault zip once the agent Gmail is connected.*


---

## Add-on (2026-10-26): Hermes Optimization — Gateway Session 2

Diego approved the full Hermes pack. Everest-side half DONE 2026-10-26:
Oracle's ideator now uses a system prompt + JSON mode + tuned options
(temp 0.7, num_ctx 8192), legacy parser as fallback, verified end-to-end.
ORTIZ half below (full doc: `goals/weekend-llm-gateway-build/files/hermes-optimization-session2-prompt.md`).
Pairs with the Fugu bake-off addendum in the same goal folder.

```
HERMES OPTIMIZATION — LiteLLM gateway Session 2.

Context: the gateway is UP in C:\Users\anjel\warren\litellm-gateway
(image ghcr.io/berriai/litellm:v1.106.3 digest-pinned, port 4000 bound to
127.0.0.1 only). Routes: "local" -> Ollama hermes3:8b-llama3.1-q8_0,
fallback "claude" -> anthropic/claude-sonnet-5-5. Master key lives ONLY
in the project .env (gitignored). ANTHROPIC_API_KEY must NEVER be set as a
Windows env var — Claude Code would pick it up over the Pro login.

STEP 0 — close Block 3 leftovers first:
- Send one real request to the "claude" model through the gateway and show
  it answering (this auth-tests the Anthropic key end to end).
- Commit the pending compose change (.env-only key sourcing).

STEP 1 — embeddings model:
- ollama pull nomic-embed-text, then confirm it lists in /api/tags and
  answers one /api/embeddings call. (~274MB; this powers the cache and
  any future ask-your-vault features.)

STEP 2 — keep Hermes warm:
- Set OLLAMA_KEEP_ALIVE=30m for the Ollama install and restart it, so the
  model stays loaded through a work session instead of cold-loading per
  call. Confirm with a timed second request (should be much faster).

STEP 3 — semantic cache on the gateway:
- Add a small Redis container to the compose stack (internal network only,
  NO published port) as the cache store.
- Enable LiteLLM caching for the "local" model only: semantic-similarity
  cache using nomic-embed-text embeddings via Ollama, similarity
  threshold 0.90, TTL 24h. Check the LiteLLM docs in-session for the exact
  config keys — do not guess silently; if semantic wiring fights back,
  land exact-match caching instead and tell me which one shipped.
- Tests: (a) send a grunt prompt, then a near-identical rephrase — the
  second must register as a cache hit; (b) send an unrelated prompt — must
  be a miss; (c) the "claude" route must never serve cached answers.

STEP 4 — Hermes job roster. New folder C:\Users\anjel\warren\hermes-jobs
with one runner, hermes_job.py, that calls the GATEWAY's "local" model
(never Ollama directly — jobs inherit the gateway's fallback + logging).
All jobs: JSON output, num_ctx 8192, per-job temperature below, and a
--dry-run flag that prints instead of writing.
  1. digest-logs (temp 0.3): read today's local fleet outputs that exist
     (master-of-coin snapshot, kalshi paper logs, power radar log) and
     write digest-YYYY-MM-DD.md — plain-language state of each fleet:
     open/closed, net, anything unusual. First-pass reading only; no
     verdicts, no recommendations.
  2. summarize (temp 0.3): input a text/transcript file, output structured
     notes JSON: topics[], key_claims[], action_items[]. (Course-Eater's
     first pass will use this.)
  3. classify-idea (temp 0.1): input Oracle idea JSON lines, output each
     idea scored 1-5 on effort, cost_to_test, and niche_fit, with a
     one-line reason. Advisory pre-scoring for the evaluator only.
- Tests: run each job once on a real sample (today's actual logs; a real
  transcript chunk if one is handy; 3 real Oracle ideas), show me the
  outputs, then STOP. Do NOT schedule anything (no Task Scheduler entries)
  until I say go.

STEP 5 — fallback drill (proves the roster can't hard-fail):
- Stop Ollama. Run digest-logs. It must still complete via the gateway's
  claude fallback. Restart Ollama. Report both timings.

Hard rules: gateway stays 127.0.0.1; no new ports published; no keys in
any file except the existing .env pattern; do not modify PaperGuard, the
kalshi-15m folder, master-of-coin, or the Everest-side Oracle code.
Paper/research only — nothing here places orders or touches wallets.

Report back: Step 0 claude-route result, cache hit/miss test results,
which cache type shipped, one sample output per job, and the drill
timings.
```
