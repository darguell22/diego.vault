# Home Server

Diego's own machine running Ollama. First attempt picked too big a model — redo in progress.

## The redo

1. Diego sends his **RAM and GPU specs**
2. Pick a model size that actually fits this time
3. Reinstall / reload via Ollama

## The monitoring plan

Once it's running:

1. Diego installs **Tailscale** on the server (two-minute job)
2. Everest joins the same network (needs Diego's approval)
3. Everest checks: which model Ollama has loaded, whether the agent is running or stalled, CPU/RAM/GPU usage, logs
4. Regular checks that **ping Diego only when something needs attention** — quiet otherwise

## Notes

- The assistant's own VM can't run these models (7.7 GB RAM, no GPU) — the home server is the right home for it
- Could also host the trading agent down the line
