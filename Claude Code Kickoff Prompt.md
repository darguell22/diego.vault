# Claude Code Kickoff Prompt — Warren Build
Prepared 2026-10-22. Tonight's laptop session: install Claude Code (vault: AI Tools Stack Setup, Step 11), then paste the prompt below into it. It does the Docker + Vibe-Trading work on your machine directly.

**Warm-up first (5 min, do yourself):** install FinceptTerminal — download the Windows x64 `.exe` from https://github.com/Fincept-Corporation/FinceptTerminal (Releases), launch it, connect the free Yahoo Finance feed, and point its AI agents at Ollama (`http://localhost:11434`, model `hermes3:8b-llama3.1-q8_0`).

Then open PowerShell, `cd` into an empty folder like `C:\Users\anjel\warren`, run `claude`, and paste this:

```
You are helping set up a paper-trading bot on this Windows 11 laptop. Context you need:
- Ollama is already installed and running at http://localhost:11434 with the model hermes3:8b-llama3.1-q8_0 (Hermes 3 8B). Verify it with `ollama list` before anything else.
- Goal: get Vibe-Trading (open-source trading agent: https://github.com/HKUDS/Vibe-Trading) running via Docker Desktop, connected to my Alpaca PAPER trading account (balance set to $100).

Work in this order:
1. Check if Docker Desktop is installed and running (`docker --version`). If it is NOT installed: you cannot install it yourself — give me the exact install steps (download from https://www.docker.com/products/docker-desktop/, keep WSL 2 backend checked, reboot when asked), then STOP and wait for me.
2. Once Docker is confirmed running: clone https://github.com/HKUDS/Vibe-Trading and follow its README's Docker "zero setup" path.
3. Configure it for PAPER trading only. When you need my Alpaca paper API key and secret, STOP and ask me to paste them — never invent keys, never use placeholders that look real.
4. Set these guardrails in its config: max position size $25; daily loss halt at -$5; total kill switch at -$15 drawdown (only I restart it after that); max 3 day-trades per 5 days (PDT protection).
5. Point its LLM brain at local Ollama: use http://host.docker.internal:11434 from inside Docker (or http://localhost:11434 if running natively), model hermes3:8b-llama3.1-q8_0.
6. Verify at the end: show me its running status/dashboard and confirm it sees the Alpaca paper account with the $100 balance.

HARD RULES, no exceptions: NEVER configure real-money / live trading keys — paper only. If any step needs a secret or password, stop and ask me. Explain each step in one short sentence before doing it, so I can follow along.
```

**After it's running:** the 2-week paper proving period starts. Go-live criteria (unchanged): profitable on paper + guardrails never breached → only then wire the $100 real account.

---

## Job 2 — Qwen3-TTS character voices (La Familia Aguacate)

Once the Warren build is done (or in a second terminal), paste this as a new Claude Code task:

```
Set up Qwen3-TTS 1.7B on this Windows 11 machine with CUDA. Steps:
1. Check Python 3.10+ is installed (`python --version`). If not, tell me to install it from python.org and stop.
2. Install the Qwen3-TTS package and download the 1.7B Base model weights (HuggingFace: Qwen/Qwen3-TTS-12Hz-1.7B-Base). This GPU has 12GB VRAM, so the full 1.7B model fits.
3. Verify it works: generate a short test line ("Hola, soy Hassito, el aguacate más verde del barrio.") with 4 different preset speakers, saving each to a separate WAV file in a folder called aguacate-voices.
4. List the preset speaker names available so I can pick one voice per character: Hassito (young boy), Papá Tomás (adult male), Mamá Rosa (adult female), Abuela Tere (older female).

Notes: everything runs locally, no API keys needed. If a step needs admin rights or a reboot, stop and tell me exactly what to do.
```

Pick the 4 voices by listening, then those become the locked character voices for all 50 episodes (multilingual re-renders later are $0).
