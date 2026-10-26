# Decision — Kalshi 15-Min: keep sub-80¢, fix the fee model (2026-10-26)

**The call:** Stage R v2 keeps sub-80¢ entries only. The 80–89¢ band and
shelving the family were both on the table; sub-80 won because it was
pre-registered before the final tally, so the test stays clean.

**Why it came up:** Recomputing all 254 v1 windows from the raw log found
two corrections:

1. The final sub-80 tally is n=76 at −4.78¢/trade, not the −3.87¢ (n=75)
   from the 248-window snapshot.
2. The tracker's flat 7¢/trade fee was wrong. Kalshi's real taker fee =
   0.07 × P × (1−P) per contract (max 1.75¢ @50¢), rounded up per fill.
   Under real fees: v1 = −0.77¢/trade (still rejected), sub-80 =
   **+0.26¢/trade**, 80–89 = +2.81¢/trade (n=26, small sample), 99–100¢ =
   0.00¢/trade (100% win, no gross edge).

**The rule going forward:** every paper tracker models the venue's REAL
fee formula, never a flat stand-in. v2's fee: ceil(0.07 × P × (1−P) × 100)
per 1-contract trade, 0 at P=100¢, entry only.

**Gate:** v2 nets ≥0¢/trade (real-fee) over ≥30 entered trades spanning
≥14 days + Diego says "graduate." Fail = hypothesis to the graveyard,
family shelved (second failure).

**Also checked:** no cheaper taker venue for interval BTC. Polymarket US
≈ identical fee (0.0695 formula; makers $0 + rebates). Robinhood/Coinbase
route to Kalshi plus extra commission. The fee lever is maker/limit
orders, not switching apps.

Full prompt: [[Claude Code Collector Prompts]] (Stage R — Kalshi section).
Ledger: goals/automate-the-kalshi-15-min-btc-strategy/hidden_files/stage-r-log.md
