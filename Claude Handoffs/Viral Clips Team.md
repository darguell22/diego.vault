# Claude handoff: Viral Clips Team
_Paste this whole note into Claude, then ask your question._

## Who
Diego, Austin TX. Money is tight — free/local-first, quote any cost before spending. Laptop: RTX 5070 Ti 12GB, 32GB RAM, Windows 11.

## The goal
An agent team that finds trending clips, adds a transformative layer (AI avatar host reacting/commenting — never straight reposts, platforms penalize those), and posts across TikTok/Shorts/Reels.

## Key decisions so far
- **Avatar host:** Open-LLM-VTuber (open-source, runs offline via Ollama). Sample Live2D models are NOT licensed for commercial use — needs a custom avatar before monetizing. v1 is maintenance-only.
- **Footage:** clipping campaigns (Vyro ~$3/1k views, FindClout) = pre-cleared footage, paid per verified view, no copyright risk.
- **AI disclosure** required on YouTube/TikTok/Meta for AI avatar content.
- Transformative case only holds with real opinions/jokes in the script — generic filler gets flagged as inauthentic.

## Current state (2026-10-21)
Pipeline map done. Not installed yet. Install path: Docker → Ollama → Open-LLM-VTuber. Niche not picked yet; Vyro/FindClout signup pending.

## Avatar host options (added 2026-10-21)
Two routes for the reacting/commenting host. Pick one before building.

| | Open-LLM-VTuber (local) | Higgsfield Soul ID (cloud) |
|---|---|---|
| Cost | $0 (runs on laptop) | ~$1.25 to train + ~$9/mo plan |
| Face consistency | Good (Live2D rig) | Excellent (trained identity, locked across clips) |
| Setup | Docker + Ollama + config | Upload 20+ face photos, 3-min train |
| Look | Anime/cartoon VTuber style | Photorealistic human |
| Catch | Sample models not licensed for commercial use — needs custom avatar; v1 maintenance-only | Their terms let them train on your inputs; vendor lock-in |
| Best if | You want $0 and an anime-style host | You want a realistic human host with zero setup |

Note: if the host is photorealistic, AI disclosure rules still apply on all platforms.

## Ask Claude for
E.g. "write 10 avatar reaction scripts for [niche] clips," "draft the clip-selection criteria," "compare Vyro vs FindClout for a beginner budget."
