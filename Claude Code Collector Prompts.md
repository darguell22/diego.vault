# Claude Code Collector Prompts

Paste-ready prompts for Claude Code on laptop ORTIZ (Claude Pro). Purpose: RELOCATE
as much as possible off Everest's cron pile (every run there burns Muse usage)
onto plain local scripts run by Windows Task Scheduler — $0 to run, forever.
Everest stays on the daily evaluation layer only, since Diego uses him a lot and
wants the usage spent on thinking, not babysitting scripts.

Common rules baked into every prompt: Python standard library ONLY (no pip installs
without asking), read-only public APIs, no auth, no API keys, no real orders ever.

Bridge note (v1): logs live on the laptop. Bring results to Everest by pasting a
summary output, or leave for the daily review when the laptop is in a session.

---

## 💰 Paper → Real Graduation Protocol (added 2026-10-24, Diego's request — applies to Prompts 1, 2, 3, 7)

**Platform stack (researched 2026-10-24):**
- Meme execution: **Axiom** (axiom.trade) — the market default (most-used Solana terminal by a wide margin), self-custody wallets, ~0.95–1% fee. ⚠️ The referral fee discount applies **only at account creation** — attach a code when signing up; it can't be added later. Auto take-profit / stop-loss + limit orders map 1:1 onto the sims' TP/SL rules; MEV protection via Jito.
- Discovery stays DEX Screener + the local sims. **GMGN** = best smart-money labels, but its generated wallet keys can't be exported — research only, never the execution wallet. **Photon** = fast, but fee reports conflict (0.25% vs 1%) — unproven. **BullX** = paused trading in June 2026 — dead, never use.
- Wallet/funding: Axiom's built-in self-custody wallet (or Phantom). Fund small when the time comes: Coinbase → SOL → wallet. Keys are created and held by Diego only — never on Everest, never inside any script, never in chat.

**The gate — ALL must be true before any real dollar moves:**
1. Paper book net positive over ≥30 closed trades AND ≥14 days at final paper settings.
2. Guardrails never breached in paper.
3. Diego explicitly says "graduate" for that specific strategy. No prompt ever flips itself to real.

**Stages:**
- **Stage 0 — paper.** Where everything is now.
- **Stage 1 — manual real.** Bankroll cap $100 total, max $5/trade, daily loss halt −$10, total kill switch −$25. Execution is MANUAL on Axiom: the sim prints the signal, Diego places the trade and sets auto TP/SL in Axiom. No bot holds a key.
- **Stage 2 — semi-auto.** Only after ≥2 weeks of Stage 1 with the book still positive, plus Diego's explicit approval. Any automation design gets a security review first. A hot wallet on an always-on machine stays against Diego's security rule unless he changes that rule himself.

**Stage R — Rebuild loop (scientific method).** Graduation is not one-way. A strategy that fails AFTER graduating gets demoted and rebuilt, never just mourned:
- **Demotion triggers (automatic, no debate in the moment):** Stage 1 kill switch hit (−$25 total), OR the Stage 1 book is net negative after 20 real trades, OR two daily halts in one week. On trigger: real trading stops, strategy returns to Stage 0 paper, and the failed config is frozen exactly as it ran.
- **The paper twin is the control group.** During Stage 1 the paper sim keeps running the same signals in parallel, so a rebuild can compare live vs paper on identical entries — slippage/execution problems separate cleanly from strategy problems.
- **Rebuild cycle, one loop at a time:**
  1. **Observe** — write down what the data says failed (which leg/source, which conditions), from the logs, not from vibes.
  2. **Hypothesize** — ONE falsifiable claim, e.g. "the FOMO leg bleeds because entries chase the first minutes of a spike; a 10-minute entry delay improves net/trade." If it can't be proven wrong by numbers, it's not a hypothesis.
  3. **Change ONE variable** — entry timing, TP/SL levels, source filter, size, time stop. Never two at once; you'd never know which one worked. Bump the version (v1.0 → v1.1) and keep the failed version's full record.
  4. **Paper test** — the new version must pass THE SAME GATE (≥30 closed trades, ≥14 days, net positive, no breaches). No shortcut for being a rebuild.
  5. **Verdict** — compare new vs old on net/trade per source, win rate, max drawdown. Better + gate passed → eligible to re-graduate (Diego says the word again; re-entry starts at Stage 1 minimums). Not better → hypothesis REJECTED, logged, next hypothesis.
- **Graveyard rule:** every rejected hypothesis goes in a ledger with its numbers. A hypothesis family that fails twice is shelved permanently — no retesting dead ideas, no tuning on fewer than 30 trades, max one active hypothesis per strategy. The daily feedback-loop evaluator acts as referee: it flags demotion triggers and tracks rebuild versions.

**Status per prompt:**
- **Prompt 2 (Master of Coin): DOES NOT QUALIFY yet** — paper book −$284.03 at 8.1% win rate. Build the graduation machinery (Prompt 2 addendum below); the money does not move until the sim turns net positive.
- **Prompts 1 & 3 (Kalshi):** same gate. Both strategies are −EV on paper so far (the 15-min tracker wins ~86% but needs ~93% to break even). Real Kalshi trading, if it ever passes, uses Diego's official Kalshi account only.
- **Prompt 7 (Power Radar):** alerts-only forever. "Graduation" = Diego manually trading a flag on his own accounts — never copy-trading automation (Whale Mirror already proved blind copying loses).

---

## Prompt 1 — Hourly Kalshi BTC paper collector (KXBTCD)

```
Build a read-only paper-trading collector for Kalshi's hourly Bitcoin
above/below markets (series ticker KXBTCD). Python standard library only —
no pip installs. No authentication, no API keys, never place orders.

First verify the series shape with the public API:
GET https://api.elections.kalshi.com/trade-api/v2/markets?series_ticker=KXBTCD&status=open&limit=5
Each hourly event (ticker like KXBTCD-26OCT2221, hour in ET = window END) has
several markets at different strikes (floor_strike, about $100 apart).

Create folder C:\Users\anjel\warren\kalshi-hourly\ with hourly_tracker.py:

Logic for one window (flag --windows 1 runs exactly one, then exits):
1. Find the live hourly event: window open = top of current hour UTC,
   close = open + 1h. Get its markets via
   GET /trade-api/v2/markets?event_ticker=<EVENT>&limit=200
2. Wait until T+50:00 into the window (10 minutes before close).
3. At entry: get BTC spot from
   https://api.coinbase.com/v2/prices/BTC-USD/spot
   Pick the market whose floor_strike is CLOSEST to spot.
   Signal = YES if spot > strike, else NO.
   Entry price = that side's ask (yes_ask_dollars or no_ask_dollars) in cents.
   If quotes or strike are missing, skip the window.
4. Wait until close + 20s, then poll GET /trade-api/v2/markets/<ticker>
   every 15s (up to 5 min) until status == finalized and result is set.
5. win = (result matches signal). gross = (100 - entry) if win else -entry.
   fee = 7 cents assumed. net = gross - fee.
6. Append a row to paper_log_hourly.csv with columns:
   ts_entry_utc, event_ticker, market_ticker, window_open_utc, window_close_utc,
   strike_usd, btc_spot_usd, signal, entry_ask_cents, yes_bid_cents,
   yes_ask_cents, no_bid_cents, no_ask_cents, result, win, gross_pnl_cents,
   fee_cents, net_pnl_cents
   Also rewrite variant-hourly.json with running totals:
   {windows, win_rate_pct, net_cents}.

Also write summary_hourly.py that reads the CSV and prints windows, win rate,
avg entry, gross/net P&L, and breakeven win rate = (avg entry + 7) / 100.

Then create a Windows Task Scheduler task "KalshiHourly" that runs
python hourly_tracker.py --windows 1 every hour at minute 2, starting in
C:\Users\anjel\warren\kalshi-hourly\. Show me the task settings before enabling.
Stop and show me the first live window's entry values before scheduling anything.
```

---

## Prompt 2 — FOMO watcher + Master of Coin paper sim (local runner)

```
Build a local paper-only meme-coin watcher and simulator. Python standard
library only — no pip installs. No wallets, no keys, no real trades, ever.

Create folder C:\Users\anjel\warren\master-of-coin\ with three scripts:

1) fomo_poll.py — GET https://fomotrendings.com/api/calls (public, no auth).
   Dedupe tokens against meme-log.jsonl in this folder by contract address;
   append new ones as JSON lines tagged "source": "fomo" with ticker, chain,
   entry_mcap, first_seen timestamp. Print {"new_count": N}.

2) launch_poll.py — GET https://api.dexscreener.com/token-profiles/latest/v1
   (public, no auth). Dedupe by contract address against the SAME
   meme-log.jsonl; append new ones tagged "source": "launch".
   Print {"new_count": N}.

3) paper_sim.py — paper simulator over both sources:
   - Opens a $10 paper position on new tokens from either source.
   - Max 10 open positions total, max 5 per source.
   - Prices from https://api.dexscreener.com/latest/dex/tokens/<address>
   - Take profit at 2x entry value, stop loss at -50%, 24h time stop,
     token vanished from DEX Screener = close at 0.
   - 2% assumed round-trip cost on every close.
   - State in open-positions.json; snapshot master-of-coin.json with combined
     totals AND per-source split: {open, closed, win_rate_pct, net_cents,
     per_source: {fomo: {...}, launch: {...}}}.

Then create a Windows Task Scheduler task "MasterOfCoin" that runs all three
(poll, poll, sim) every 2 minutes. Show me the task before enabling it, and
show me the first run's output before we call it done.
```

### Migration seed — Everest state at shutdown (2026-10-23 ~11:52 CT)

The Everest-side watcher was killed 2026-10-23; the sim migrates here. Final Everest snapshot (`master-of-coin.json`):

- **Combined:** open 10 · closed 37 · win rate 8.1% · net −28,403¢ (−$284.03), net/trade −768¢
- **FOMO leg:** open 5 · closed 33 · win 6.1% · net −27,007¢
- **Launch leg:** open 5 · closed 4 · win 25.0% · net −1,396¢
- **Meme-log size at shutdown:** 426 tokens logged (`meme-log.jsonl`)
- Rules in force: $10/trade, max 10 open shared, max 5/source, 2x TP / −50% SL / 24h time stop, vanished = close at 0, 2% round-trip cost.

**Open positions at shutdown** (ticker · source · chain · entry mcap · opened UTC · address):

Launch leg:
- DSP · launch · base · 147,775 · 2026-10-22T21:15:42Z · 0x0039D6a1773888cbe38adDeeB2054FE145333DFB
- Motion · launch · solana · 3,243.77 · 2026-10-22T23:02:28Z · 7mwXd6HDxytJmWRZnNLoaVJjNh2XFrVWGq6GTvXopump
- Christine · launch · solana · 4,059.07 · 2026-10-22T23:10:34Z · EK12A4bPsfSSWcfcYxS5hDXyFwmnPxzwuGZ4Exivpump
- LI · launch · solana · 4,336.18 · 2026-10-22T23:49:05Z · 13yEoWFGBr5GzedAuC6jwu4wbwPBbSwTPG4wsGmnpump
- Humor · launch · solana · 6,200.19 · 2026-10-23T03:53:29Z · 2cHK7BzK6ztLidqqQC9vfiuG7j3owbtjebJiiRDgpump

FOMO leg (all opened 2026-10-23T16:37:57Z):
- Kimchi · fomo · solana · 38,157.99 · 1RjmyJbkSeQFd2fmc2LBhnPF45dSGGBzNGo6bw9pump
- NIO · fomo · solana · 212,962.29 · 7SL2VnfYsChvog5oQY9Nmtqu1wceaMiQzWoqoLQpump
- McRib · fomo · solana · 898,377.33 · 8idEA8QovsZJka6d9Z35fejE5MbL37BJxvw5k5r6pump
- 4 · fomo · bsc · 13,513,721.10 · 0x0a43fc31a73013089df59194872ecae4cae14444
- BIBI · fomo · bsc · 266,099.99 · 0x9bafb44d7cc9394465a96ef0637f6cbce8e47777

**Instruction:** When building the local runner, initialize its state files from this seed so open positions keep their original entries; if the local format differs, start the local sim fresh and treat this seed as the historical record — do not double-count.

### Prompt 2 addendum — graduation machinery (paste AFTER Prompt 2 is built and running)

```
Add a graduation module to this Master of Coin runner. Context: the paper sim
may one day graduate to small real-money MANUAL trading on Axiom under the
Paper → Real Graduation Protocol in this file. The runner itself must NEVER
place orders or touch a wallet. Build:

1. graduation_check.py — reads the sim's state and prints a checklist:
   closed trades, days running at current settings, net P&L (total and split
   fomo vs launch), win rate, guardrail breaches. Marks each gate PASS/FAIL:
   (a) ≥30 closed trades, (b) ≥14 days, (c) net P&L positive, (d) zero
   guardrail breaches.
2. A config flag REAL_MODE, default false. When REAL_MODE is true AND all
   gates pass, the runner also writes each new paper entry to
   signals_live.jsonl (token, chain, entry price/mcap, TP/SL levels per the
   sim rules, suggested size capped at $5) — Diego's manual execution list
   for Axiom. It places NO orders and holds NO keys.
3. If REAL_MODE is true but any gate fails, print "GRADUATION GATE: BLOCKED"
   with the failing items and write nothing to signals_live.jsonl.
4. A simple stage1_tally.csv that Diego edits by hand to mark which signals
   he actually took and their results; graduation_check.py reads it and shows
   the Stage 1 running P&L against the caps ($100 bankroll, −$10/day halt,
   −$25 total kill switch).

5. Rebuild support (Stage R in the Graduation Protocol): a rebuilds.jsonl
   ledger — one line per version: version number, the single variable changed,
   the falsifiable hypothesis in one sentence, gate results, and the verdict
   (graduated / rejected / shelved). graduation_check.py prints the current
   version, its hypothesis, and the graveyard count of rejected hypotheses.
   During Stage 1 the paper sim keeps running as the control group, and the
   check script reports live-vs-paper on identical signals side by side.

No new network calls. No wallet libraries, no signing, no order placement —
ever. Show me a sample graduation_check output when done.
```

---

## Prompt 3 — 15-min Kalshi tracker (local clone) ⚠️ migration, not a copy

Only run this when migrating the 15-min tracker off Everest entirely.
If this runs locally, tell Everest to switch his copy OFF — two trackers
logging the same strategy in two places corrupts the data.

```
Build a local clone of a Kalshi 15-minute BTC paper tracker. Python standard
library only — no pip installs. Read-only public APIs, no auth, no orders.

Create folder C:\Users\anjel\warren\kalshi-15m\ with tracker.py:

Series KXBTC15M ("BTC Up or Down - 15 minutes"), one market per event.
Event ticker form: KXBTC15M-YYMONDDHHMM (HHMM = window END in ET).
For each window: wait until T+12:30 in (2.5 min before close). At entry:
strike from the market's yes_sub_title ("Target Price: $X"), BTC spot from
https://api.coinbase.com/v2/prices/BTC-USD/spot
Signal = YES if spot > strike else NO. Entry = signaled side's ask in cents
(yes_ask_dollars / no_ask_dollars). After close + 20s, poll the market until
status == finalized; win if result matches signal; gross = (100 - entry) if
win else -entry; fee 7 cents; net = gross - fee. Append to paper_log.csv
with columns: ts_entry_utc, event_ticker, market_ticker, window_open_utc,
window_close_utc, strike_usd, btc_spot_usd, signal, entry_ask_cents,
yes_bid_cents, yes_ask_cents, no_bid_cents, no_ask_cents, result, win,
gross_pnl_cents, fee_cents, net_pnl_cents.
Flag --windows 1 runs one window then exits.

Also summary.py (windows, win rate, avg entry, gross/net, breakeven).
Then a Task Scheduler task "Kalshi15M" running tracker.py --windows 1 every
15 minutes starting at minute 1. Show me everything before enabling.
```

---

## Prompt 4 — Warren Command Dashboard (local edition)

Builds the dashboard on ORTIZ from the local collectors' files. Panels whose
feeds still live on Everest (All In/slips, Whale Mirror, Oracle) render as
reserved dark bays — no fake data, same rule as the original.

```
Build a local "Warren Command Dashboard" — a single HTML page that renders
the state of my paper-trading collectors from local files. Python standard
library only for the builder script. No network calls except localhost
Ollama. No keys, no orders, no real trades anywhere.

Create folder C:\Users\anjel\warren\dashboard\ with build_dashboard.py that
reads these local data sources (if a file is missing, render that panel as
NO FEED — never invent numbers):
- ..\kalshi-15m\paper_log.csv — 15-min Kalshi paper log (may not exist yet)
- ..\kalshi-hourly\paper_log_hourly.csv and ..\kalshi-hourly\variant-hourly.json
  (variant totals: {windows, win_rate_pct, net_cents})
- ..\master-of-coin\master-of-coin.json — {open, closed, win_rate_pct,
  net_cents, per_source: {fomo: {...}, launch: {...}}}
- ..\master-of-coin\open-positions.json — open paper positions with tickers
- ..\master-of-coin\meme-log.jsonl — last 15 lines feed the MEME RADAR panel
- Brain status: GET http://localhost:11434/api/ps and /api/tags (Ollama).
  If unreachable, the brain panel shows OFFLINE.

Compute per-panel stats from the CSVs inside the builder (windows, win
rate, avg entry, gross/net cents, breakeven win rate = (avg entry + 7)/100)
and embed everything into ONE self-contained file warren-dashboard.html —
inline CSS/JS, no external assets. Look: dark CRT terminal, black
background, phosphor green text (#33ff66 family), monospace, subtle
scanlines, header "WARREN COMMAND — STN-01", a "generated at" timestamp.

Panels:
1. BRAIN — Ollama status, loaded model name/size, or OFFLINE
2. GUARDRAILS — static: Alpaca PAPER only · $25 max position · $5 daily
   loss halt · -$15 drawdown kill switch · PDT protection · 2-week paper
   proving before real dollars
3. KALSHI 15-MIN — stats from the 15m log, or NO LOCAL FEED
4. KALSHI HOURLY — stats from the hourly log + variant totals
5. MASTER OF COIN — open/closed/win rate/net, per-source fomo vs launch
   split, open position tickers
6. MEME RADAR — latest tokens from meme-log.jsonl
7. Three dark placeholder bays: ALL IN · WHALE MIRROR · ORACLE, labeled
   "FEED RUNS ON EVEREST — NOT LOCAL". Reserved only; no fake data.

Then create a Windows Task Scheduler task "WarrenDashboard" that runs
build_dashboard.py every 5 minutes so the page stays fresh. Show me the
rendered panel list and the task settings before enabling anything.
```

---

## Prompt 5 — Course-Eater (pointer)

Not a collector — this one lives in its own note:
**[[Claude Code Course Eater Prompt]]** (same vault folder).

It builds a local pipeline on ORTIZ (C:\Users\anjel\warren\course-eater\)
that takes a course Diego owns — lesson files or transcripts he provides
himself, no logins or paywall tricks — transcribes it locally with
faster-whisper on the GPU ($0, offline), extracts per-lesson skills into
structured notes, audits them against his current workflows (YouTube
explainer pipeline, viral clips, Aguacate), and produces one ranked
integration plan. Personal study notes only; any course download/script
gets flagged for a SkillSpector scan before it is ever run. Run it when a
course is actually on deck — it stops after a first sample lesson for
review before batch-processing anything.

---

## Prompt 6 — BTC cycle tracker (local clone) ⚠️ migration, not a copy

Diego approved this build 2026-10-23 (after the 4chan cycle-template
discussion — the post called the Oct 6, 2025 ATH to the day, then its
bottom call missed by ~3 months; this tracker keeps the scoreboard).
Only run this when migrating the cycle tracker off Everest entirely.
If this runs locally, tell Everest to switch his copy OFF — two trackers
appending to two logs in two places corrupts the scoreboard
(same migration rule as Prompt 3).

```
Build a local, paper-only Bitcoin cycle/regime tracker. Python standard
library only — no pip installs. Read-only public APIs, no auth, no API
keys, never place orders. This measures cycle position; it does not
trade and does not predict price.

Create folder C:\Users\anjel\warren\btc-cycle\ with cycle_tracker.py:

Data (Coinbase public, no auth):
- Spot: GET https://api.coinbase.com/v2/prices/BTC-USD/spot
- Daily history: GET https://api.exchange.coinbase.com/products/
  BTC-USD/candles?granularity=86400 with start/end params, paginated
  300 candles per request, back to 2020-01-01. If a page fails, continue
  with what was retrieved and note it.

Hardcoded known facts:
- Halvings: 2012-11-28, 2016-07-09, 2020-05-11, 2024-04-20.
  Next halving estimate = 2024-04-20 + 1460 days (~Apr 2028, label EST).
- Prior cycle ATHs: 2017-12-17 ~$19.8K, 2021-11-10 ~$69K,
  2025-10-06 $126,210 (verified). If the history max disagrees with
  $126,210, prefer the larger and note the source.

Compute: spot, ATH (price+date), % below ATH, days since ATH, cycle low
since ATH (price, date, % drawdown at low), days since last halving,
days until next halving (est), 200-day moving average, Mayer Multiple
(spot/200DMA), price vs 200DMA %, and a cycle PHASE from this exact
ruleset (first match wins):
  1. LATE CYCLE                 Mayer > 2.4
  2. MARKUP/BULL                spot within 20% of ATH
  3. POST-ATH BEAR              <=365d since ATH and spot below 200DMA
  4. CAPITULATION/ACCUMULATION  drawdown from ATH worse than -50%
  5. RECOVERY                   spot above 200DMA
  6. BEAR/DECLINE               otherwise

4chan-template scoreboard: template intervals are ATL->ATH 1064d and
ATH->ATL 364d. ATH printed 2025-10-06, so template predicted the cycle
low on 2026-10-05. Report actual low date vs template date and the
signed error in days (Everest's copy measured the actual low on
2026-06-30, error -97d — the low came early).

Outputs: append one row per run-day to cycle_log.csv (date, spot, ath,
pct_from_ath, days_since_ath, low_since_ath, days_since_halving,
mayer, phase) — re-running on the same day replaces that day's row,
never duplicates. Also write a btc-cycle.json snapshot with every
computed field plus generated_at, and print a one-line summary.

Then create a Windows Task Scheduler task "BTCCycle" that runs
python cycle_tracker.py once daily. Show me the task settings and the
first run's summary line before enabling anything.
```

## Prompt 7 — Power Radar (local clone) ⚠️ migration, not a copy

Diego commissioned Power Radar 2026-10-23 (after the Hegseth /
prediction-market TikTok): watch the public footprints of big money and
people of power. Everest's v1 is live and verified. Only run this when
migrating Power Radar off Everest entirely — if it runs locally, tell
Everest to switch his copy OFF the same night (same rule as Prompts
3 and 6; two radars double-log alerts).

```
Build a local, alerts-only "Power Radar". Python standard library only
— no pip installs. Read-only public APIs, no auth, no API keys, never
place orders, never connect a wallet. It watches public data and flags
patterns; it does not trade and does not copy trades.

Create folder C:\Users\anjel\warren\power-radar\ with power_radar.py:

LEG A — Polymarket big-trade radar:
- GET https://data-api.polymarket.com/trades with params limit=200,
  filterType=CASH, filterAmount=10000. The server-side CASH filter is
  essential: the unfiltered feed is 5-minute BTC micro-trades (1,000
  trades can span ~1 minute, max under $5k).
- Trade USD value = size * price. Log every trade >= $10,000.
- Flag BIG_POWER: >= $25,000 on a power-category market. Power
  keywords (match against title + slug, lowercased): trump, biden,
  harris, election, president, congress, senate, house passes,
  shutdown, iran, israel, gaza, russia, ukraine, putin, zelensky,
  china, taiwan, " war ", "war?", world war, ceasefire, strike on,
  military, nato, "fed ", interest rate, powell, maduro, venezuela,
  regime, impeach, tariff, executive order, supreme court, midterm,
  white house, pentagon, nuclear. NEVER use a bare "war" keyword —
  it false-fires on "Warriors" and "Delaware".
- Flag BIG_ANY: >= $50,000 on any market.
- Flag INSIDER_PATTERN: a BIG_POWER trade whose wallet has a genuinely
  small lifetime history. Profile the wallet via
  GET /trades?user=<proxyWallet>&limit=500. Fresh = trade count < 500
  AND (count <= 20 OR (count <= 100 AND earliest fetched trade is
  <= 7 days old)). A wallet AT the 500 cap is a bot/market-maker,
  never fresh. Cap wallet lookups at 5 per run. A flag is a pattern
  match, NOT proof of insider trading — label it that way.
- Account age on EVERY power flag (added 2026-10-23 at Diego's
  request): also GET https://gamma-api.polymarket.com/public-profile
  ?address=<wallet> and record createdAt, wallet age in days, lifetime
  trade count, and volume of fetched trades on the alert. A big bet
  from a 9-month-old $1M-volume wallet reads as conviction; the same
  bet from a wallet created today reads as the insider pattern.

LEG B — SEC insider filings:
- GET https://www.sec.gov/cgi-bin/browse-edgar?action=getcurrent
  &type=4&dateb=&owner=include&count=100&output=atom with a descriptive
  User-Agent header. Parse the Atom feed; each entry is a Form 4
  (officer/director/10%-owner transaction filing). Log new entries at
  INFO level — they are routine, not alarms.

State and outputs: seen.json (dedupe by Polymarket tx hash + asset,
and by EDGAR entry id; keep bounded), alerts.jsonl (append one JSON
object per flagged/logged item), power-radar.json snapshot (last run
time, counts, flags, errors). Print a short summary per run. First run
reports the current window as the first sweep; later runs report only
new items. If a leg fails, note the error in the snapshot and exit
non-zero, but still write the other leg's results.

Do NOT add Congress trades, crypto whale wallets, or Kalshi identity
tracking — no free reliable source exists for them (House Stock
Watcher is 403; Kalshi identities are not public). Leave them out
rather than faking them.

Then create a Windows Task Scheduler task "PowerRadar" that runs
python power_radar.py every 30 minutes. Show me the first run's
summary before enabling the task.
```


---

## Stage R — Master of Coin v2: Exit Rules (added 2026-10-26)

Diego's decision, 2026-10-26. v1 frozen as control (37 closed, 8.1% win, net
−28,403¢). Feed analysis: 27% of watched coins touched 2x, median final 0.97x —
exits are the leak. v2 changes ONE variable (exit policy), entries byte-identical.
Full doc: `goals/warren-trading-bot-build/files/master-of-coin-stage-r-v2-prompt.md`
(analysis: that goal's hidden_files/master-of-coin-feed-analysis-2026-10-26.md).
Gate: net ≥0¢/trade over ≥30 closed trades spanning ≥14 days, Diego says
"graduate."

```
STAGE R REBUILD — Master of Coin paper sim, variant v2_exits.

Context: v1 (entries from the shared meme log, hold to the existing close
rules) is frozen as the control: 37 closed, 8.1% win, net -28,403c.
Do not modify paper_sim.py, v1's state files, or its history.

First, read paper_sim.py, the launch/FOMO pollers, and the current state
files so v2 matches v1's mechanics exactly. Then build v2 alongside v1:

Files:
- NEW: paper_sim_v2.py, state file master-of-coin-v2.json, trade log
  trades_v2.jsonl — all in the master-of-coin folder.
- v2 reads the SAME shared meme-log.jsonl the pollers already maintain.
  Do not add any new DEX Screener or FOMO polling for v2 — no double-hitting
  the APIs; v2 consumes the feed v1's pollers produce.

Entries — byte-for-byte v1 behavior:
- Same two legs (fomo + launch), same entry triggers, same position sizing,
  same caps inside v2's own book: max 10 open total, max 5 per source.
- Same price source and the same fee/slippage assumptions as v1 (if v1
  models none, v2 models none — parity with the control matters more
  than realism for this A/B).

THE ONE VARIABLE — exit policy, checked on the same price updates v1 uses.
"Multiple" = current mcap / entry mcap; "peak" = highest multiple the
position has reached; age = minutes since entry.
1. STOP: multiple <= 0.50 -> close the whole position. Reason: STOP.
2. TP1: multiple first reaches >= 2.00 -> sell 50% of the position.
   Reason: TP1. (Fires once per position.)
3. TRAIL: after TP1 has fired, if multiple <= 0.60 x peak -> close the
   remainder. Reason: TRAIL.
4. TIME: age >= 60 and peak < 1.50 -> close the whole position.
   Reason: TIME.
5. MAXHOLD: age >= 240 -> close everything still open, any state.
   Reason: MAXHOLD.
Rules are evaluated in the order STOP, TP1, TRAIL, TIME, MAXHOLD on each
price check; a position closed by an earlier rule is done.

Accounting and logging:
- Every close event (partial or full) appends one row to trades_v2.jsonl:
  address, ticker, source leg, entry mcap, event time, age_min, multiple
  at event, peak multiple, fraction closed, reason.
- A position counts as ONE closed trade when fully exited. Its realized
  multiple is the size-weighted average of its exit multiples, and its
  capture ratio = realized multiple / peak multiple.
- The v2 snapshot (master-of-coin-v2.json) mirrors v1's shape plus:
  per-trade capture ratios and a running median capture ratio.

Summary:
- Add a v2 mode to the existing summary (or summary_v2.py) printing
  v1 vs v2 side by side: closed trades, win rate, net cents, net
  cents/trade, median capture ratio (v2 only).

Tests — run these BEFORE wiring v2 into any schedule, and show me the
results plus a one-screen v1-vs-v2 config diff:
(a) Path 1.0 -> 2.2 -> 1.2: TP1 sells half at 2.0; TRAIL closes the rest
    (0.60 x 2.2 = 1.32, and 1.2 <= 1.32).
(b) Path 1.0 -> 0.45: STOP closes all.
(c) Flat path peaking at 1.2 for 70 minutes: TIME closes all at 60.
(d) Slow grind reaching only 1.3 by 240 minutes: MAXHOLD closes all.
Then STOP and wait for my "go" before adding v2 to the scheduled run.

Paper only — no orders, no keys, no wallet access. Do not touch the
Everest dashboard actions (update_master_of_coin stays v1-only), the
kalshi-15m folder, PaperGuard, or the LiteLLM gateway.

The test: first >=30 closed v2 trades over >=14 days, net >= 0c/trade.
```


---

## Stage R — Kalshi 15-Min v2: Sub-80¢ Entries + Corrected Fee Model (added 2026-10-26)

Diego's decision, 2026-10-25; confirmed 2026-10-26 after two corrections.
v1 frozen as control (254 windows, 85.8% win, gross +92¢). FEE CORRECTION:
v1's tracker charged a flat 7¢/trade — NOT Kalshi's real fee. Real taker fee
= 0.07 × P × (1−P) per contract (max 1.75¢ @50¢), rounded up per fill.
Real-fee v1 = −0.77¢/trade (still rejected); sub-80 slice = +0.26¢/trade
(n=76). Diego was offered keep sub-80 / switch to 80–89¢ (+2.81¢, n=26) /
shelve, and chose KEEP SUB-80 with the fee model fixed in v2 — sub-80 was
pre-registered, so the test stays clean. v2 changes ONE variable (entry
filter <80¢) and models the REAL fee: fee_cents = ceil(0.07 × P × (1−P)
× 100) per 1-contract trade, 0 at P=100¢, entry only.
Full doc: `goals/automate-the-kalshi-15-min-btc-strategy/files/stage-r-sub80-prompt.md`
(ledger: that goal's hidden_files/stage-r-log.md).
Gate: net ≥0¢/trade (real-fee) over ≥30 entered trades spanning ≥14 days,
Diego says "graduate." If v2 fails, family is shelved (second failure).
Also checked 2026-10-26: no cheaper taker venue for interval BTC —
Polymarket US ≈ identical (0.0695 formula, makers $0 + rebates, BTC
5m/15m/hourly); Robinhood/Coinbase route to Kalshi + extra commission;
the fee lever is maker/limit orders, not switching apps.

```
STAGE R REBUILD — Kalshi 15M tracker, variant v2_sub80.

Context: the v1 strategy (enter at any price) is rejected and frozen —
254 windows, 85.8% win, gross +92c; net -$16.86 under its old (wrong) flat
7c fee model, -$1.96 under Kalshi's real fee formula below. Do not modify
v1's historical log (paper_log.csv) or its stats.

Change ONE variable only: the entry filter.
- Everything else stays byte-for-byte in behavior: same event/market lookup
  (KXBTC15M), entry read at T+12:30, strike from yes_sub_title, spot from
  Coinbase, signal = YES if spot > strike else NO, settlement polling from
  close + 20s, same fractional-cent rounding and skip rules.
- FEE MODEL — CORRECTED, this replaces v1's flat 7c/trade (which was wrong;
  do NOT reuse it): Kalshi's real taker fee per contract =
  round UP to the next whole cent of 0.07 x P x (1 - P) dollars, where
  P = entry ask in dollars. The paper sim trades 1 contract per window, so
  fee_cents = ceil(0.07 x P x (1 - P) x 100), and fee = 0 when P = 100c.
  Worked examples: 50c -> 2c (raw 1.75c rounds up), 65c -> 2c (raw 1.59c),
  80c -> 2c (raw 1.12c), 90c -> 1c (raw 0.63c), 99c -> 1c (raw 0.07c).
  Entry fee only; no settlement fee. Log this fee in the fee_cents column.
- NEW RULE: enter only if the signaled side's ask is strictly < 80 cents.
  If the ask is >= 80, do NOT enter — but still follow the window to
  finalization and log it as a skipped control row (below).

Logging:
- Write v2 rows to a NEW file, paper_log_v2.csv, in the kalshi-15m folder.
  Never append v2 rows to paper_log.csv.
- Columns: same as paper_log.csv, plus two final columns: variant, entered.
  variant = "v2_sub80" on every row. entered = 1 (taken) or 0 (cap-skipped).
- Skipped rows: fill strike, spot, signal, all four quotes, entry_ask_cents
  (the signaled ask that triggered the skip), result, win (the would-be
  outcome), and the hypothetical gross/fee/net — this is the control series,
  so v1's would-be performance stays reconstructable from the same file.

Summary:
- summary.py with no args keeps reporting v1 (paper_log.csv) as before.
- summary.py v2 reports paper_log_v2.csv: entered trades, win rate, avg
  entry, gross/net, breakeven — plus a control line for skipped windows
  (count + hypothetical net).

Scheduler:
- The existing Kalshi15M task keeps its cadence and now runs the v2 logic.
- Show me the tracker.py diff before re-enabling anything you disable.

The test: first >=30 entered trades over >=14 days, net >= 0c/trade.
Paper only — no orders, no auth, read-only APIs, same as v1.
```
