# Strawberry Prompts — Proper Papers Smoke Shop Outreach
Prepared 2026-10-22. Paste each into a Strawberry companion. Strawberry does the research; **you approve and send everything** — it never sends on its own.

Your context (already baked into each prompt): you're Diego, Texas sales rep for Proper Papers (Dope As Yola's luxury rolling papers). Pitch: rice paper, chalk-free, bleach-free, no added chemicals. Shops order via the wholesale form at properpapers.com and put "Diego" in the "Do you have an existing sales rep?" field. Your contact: 956-346-1891 | darguell22@gmail.com | IG @Diego.c.arguelles.

**Ground rules for all three jobs:** public websites only — do NOT log into any account (no Gmail, no Instagram login, no bank/brokerage). Do not send emails, DMs, or texts. Research and draft only.

---

## Prompt 1 — Find new shops in fresh Texas cities

```
You are a lead researcher for a wholesale sales rep. Find 30 smoke shops, vape shops, and tobacco shops in Texas that could carry premium rolling papers — 5 each from these cities: Arlington, Plano, Laredo, Amarillo, Waco, College Station.

These cities are already covered, so SKIP any shop in: Houston, El Paso, Dallas, Fort Worth, San Antonio, Brownsville, Austin, Round Rock, San Marcos, Corpus Christi, Lubbock, McAllen, Harlingen, Cedar Park.

For each shop find: shop name, street address, city, phone number, email address (from their website contact page, Google listing, or social bio — mark "not found" if none), and website or Instagram URL. Prefer independent shops over big chains. Skip shops that are permanently closed.

Output as a markdown table with columns: Shop | Address | City | Phone | Email | Website/IG. After the table, list any shops where you could not find an email.
```

## Prompt 2 — Find emails for phone-only shops

```
You are a contact researcher for a wholesale sales rep. For each shop below, find a working email address (check their website contact page, Google business listing, and Facebook/Instagram bio). These are all in the Pflugerville/Austin TX area.

1. Happy Town Vape & Smoke Shop — 512-368-4105
2. Vapor Station Smoke Shop — 512-350-2708
3. Wizard Hat Smoke Shop — 512-956-4849
4. Knuckleheads Smoke N Brew — 512-251-6847
5. VapeX Smoke Shop — 512-215-9244
6. Hot Spot Smoke Shop — 512-551-9728
7. Cigar and Tobacco Empire — (737) 222-5515
8. East Pecan Cigars — (512) 909-1002
9. Vape Zone — (512) 243-8116
10. Lazydaze Pflugerville — (512) 215-9388
11. Sky Hookah — (512) 580-1900
12. Chillax CBD — (512) 200-3877
13. Blackhawk Liquor & Gifts — (512) 252-8993
14. High Society Relief Dispensary Cafe — (512) 800-3543
15. Vintage Vapor — (512) 961-7663
16. Vape Pro — (979) 716-0780
17. AMP Vapor — (no phone on file)
18. Star Vapor Texas — (no phone on file)
19. Wick It Vapor / Green Apothix — (no phone on file)

SKIP these three (do not research them): Discount Smokeland (permanently closed), American Vapor Company (B2B distributor, not a retail shop).

Output as a markdown table: Shop | Phone | Email found | Where you found it (URL). Mark "not found" clearly where no email exists — do not guess or invent email addresses.
```

## Prompt 3 — Draft follow-up emails for shops already pitched

```
You are a sales copywriter for Diego, the Texas sales rep for Proper Papers (Dope As Yola's luxury rolling paper brand — rice paper, chalk-free, bleach-free, no added chemicals; shops order at properpapers.com and put "Diego" in the rep field).

On October 21, Diego emailed these shops introducing Proper Papers and proposing a 15-minute Zoom. None have replied yet. For EACH shop below:
1. Quickly research the shop (their website and Instagram) and find one specific, genuine detail to reference — something they carry, their vibe, a second location, an event they ran, etc. Never invent details; if you can't find anything specific, reference their city instead.
2. Draft a short follow-up email (under 120 words): friendly bump, reference the Oct 21 email, include the specific detail, one soft call to action (reply or 15-min Zoom), sign off as Diego with 956-346-1891 | darguell22@gmail.com.

Shops:
- Cali Vibes Smoke Shop (Houston)
- Green Market (Houston; note: also has Dallas and Fort Worth locations — one draft covering all)
- El Sol Cigars (Dallas)
- Fly High Smoke & Vape (Dallas)
- Good Karma Cigar Fort Worth (Fort Worth)
- World of Smoke & Vape, 5th St (Fort Worth)
- Artisan Vapor (Fort Worth)
- World of Smoke and Vape / Bledsoe (Fort Worth — one draft covering both)
- TX Smoke Cigars (Fort Worth)
- Smoke To Live Vapor and Smoke Shop (San Antonio — one draft covering both locations)
- Blaze Bros Smoke Shop (San Antonio)
- Yandell Smoke Shop, Fort Smoke Shop, Fox Plaza Smoke Shop, Montana Smoke Shop (El Paso — one draft covering all four)
- SVT Smoke Shop, Vapes & Tattoo Supplies (El Paso)

Do NOT draft for Smokis (Dallas) — their email bounced; Diego is handling them by phone/text instead.

Output each draft with the shop name(s) as a header, then subject line, then the email body. Do not send anything.
```

---

## After Strawberry finishes
- Prompt 1 → new rows for your 182-shop list (dedupe against it first — ask Everest to merge).
- Prompt 2 → fills the email gaps in your Pflugerville sheet; those shops become emailable.
- Prompt 3 → you review each draft, tweak, and send from Gmail yourself.
- Bring the results back to Everest to file them properly.
