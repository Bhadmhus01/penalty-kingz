# Lagos Life submission — Naija Penalty Kingz

## Dashboard copy (paste-ready)

**Game title:** Naija Penalty Kingz: 1v1 Super Striker & Keeper Showdown  
**Intended hosting platform:** Lagos Life (`https://lagoslife.app`); platform listing and approval are not verified by this repository.<br>
**Short description:** Take the spot kick. Own the Lagos night. Five fast rounds of penalty drama, Nigerian stadiums, skill upgrades and matchday rewards.  
**Full description:** Naija Penalty Kingz is an independently developed mobile-first 1v1 football penalty game. Pick your shot style, read the keeper, bend the ball into a corner, then dive to deny your rival. Play Ajegunle, Onikan, Uyo and Abuja, upgrade four player attributes, and chase the Matchday leaderboard. It includes responsive Canvas gameplay, local recovery, guest/Vatar-backed cloud progress where configured, shareable result cards, and a Nigerian matchday soundscape. Lagos Life is referenced only as the intended hosting platform; no endorsement or acceptance is claimed.<br>
**Category:** Sports / Football  
**Orientation:** Portrait; optimized for 375 × 812 and responsive mobile screens  
**Launch URL:** `https://<your-render-service>.onrender.com/`  
**Allowed embedding origins:** `https://lagoslife.app`, `https://www.lagoslife.app`, `https://naijalife.app`, `https://www.naijalife.app`, `https://lagoslife.eliysites.com`  
**Health check:** `/healthz`  
**Developer checklist:** `/api/dev-checklist`  
**Suggested tags:** Nigeria, Lagos, football, penalties, sports, casual, skill, Super Eagles

## Retention, daily challenge, and leaderboard

The app provides an Africa/Lagos timezone daily challenge (`GET /api/challenge`), a local streak and match-history record, PWA install/offline shell, and downloadable/share-sheet result cards. The daily banner does not claim to be a live fixture feed. To show a genuine scheduled matchday bonus, configure `SUPER_EAGLES_MATCH_DATES` as comma-separated `YYYY-MM-DD` dates in Lagos local time and optionally set `MATCHDAY_EVENT_TITLE` in Render; update dates from an authoritative fixture calendar. Without that configuration the game clearly labels the challenge as a daily rotation.

`GET /api/leaderboard` aggregates only signed match settlements and returns whether `VATAR_RESULTS_SECRET` is configured plus the number of accepted settlements. The UI distinguishes an unconfigured board from a configured board awaiting its first signed results; it never fabricates entries. Configure and test the platform's signed score callback using the five-line settlement signature described below before advertising a live competitive leaderboard. Guest progress is stored under a random per-browser anonymous ID and backed up to the Render disk. The header shows whether progress is cloud-saved for this browser, linked to a verified Vatar account, or only saved locally while offline. Where Vatar provides a session token and verified account ID, `/session/verify` mints an eight-hour HMAC-signed player proof and sync binds to that verified ID. Account login itself remains controlled by the host platform; this game does not invent a login flow or claim shared currencies. Paid inventory and permanent perks are reconstructed from the backend's verified purchase ledger rather than trusted client backups, and signed match settlements apply the Captain Pass leaderboard multiplier server-side.

## Games hub entry points and deep links

Use the Render service root as the Games catalog launch URL: `https://<your-render-service>.onrender.com/`. The game also supports stable in-app routes for `/?screen=home`, `/?screen=stadiums`, `/?screen=shop`, `/?screen=stats`, and `/?screen=leaderboard`; shared rival challenges use `/?rivalScore=<0-6>`. The screenshot assets are in `public/assets/shot-1.png` through `shot-4.png`, and the app permits embedding from the listed Lagos Life origins. Register the root URL as the catalog tile and use the deep links where the hub supports sub-pages. No undocumented hub-navigation SDK method or custom scheme is assumed; Lagos Life must confirm its catalog registration contract and any required callback/route before acceptance.

## Vatar SDK & server setup

The game loads `https://lagoslife.app/sdk/vatar-games-v1.js` and calls `window.Vatar.init()` once during boot, consuming its `{ player, session, mode }` result. It registers `Vatar.onPurchase`, recovers pending purchases with `Vatar.purchases()`, submits scores through `Vatar.submitScore()`, and uses `Vatar.leaderboard()` in the Games hub. Checkout is initiated through `Vatar.purchase(productId)`; a client receipt is never enough to grant items because `/claim` verifies a fresh session and independently checks the purchase owner, product, status, and configured catalog price against the Vatar API. The server API base is `https://lagoslife.app/api/games/v1`, authenticated with `Authorization: Bearer <VATAR_SECRET>`; the secret is server-only. Configure the Vatar dashboard webhook URL as `https://<your-render-service>.onrender.com/vatar-webhook`. Set `VATAR_SECRET`, `VATAR_WEBHOOK_SECRET`, `VATAR_RESULTS_SECRET`, and `PLAYER_SESSION_SECRET` in Render before publishing. Render generates `PLAYER_SESSION_SECRET` through the Blueprint unless you override it. Session verification is `POST /session/verify` with `{ session }`; purchase lookup is `GET /purchases/{purchaseId}`; delivery acknowledgement is `POST /purchases/{purchaseId}/deliver` with `{}`. The webhook requires `X-Vatar-Signature: sha256=<hex HMAC-SHA256>` using `VATAR_WEBHOOK_SECRET`, and paid-purchase webhook events are checked against Vatar before delivery. Purchase fulfillment is `POST /claim` (also `/api/claim`). Signed result endpoint: `POST /api/match/settle`. Its `X-Vatar-Signature` is `v1=<hex HMAC-SHA256>` of exactly five UTF-8 newline-separated fields, in order: `matchId`, `playerId`, `score`, `opponentScore`, `timestamp`.

> Configure Render disk/persistent storage for `/opt/render/project/src/data` for durable progress across instance restarts. Client-side `clientBackup` high-water marks provide recovery when a player returns from browser storage. The free Render filesystem is ephemeral without a persistent disk.

## Product catalog

| Product ID | Price | Type | Catalog name | Benefit |
|---|---:|---|---|---|
| `curve_boots_100` | ₦100 | consumable | Fireball Curve Boots Pack | +500 Golden Balls & 3 Fireball Curve Shots (guaranteed top-corner bend) |
| `keeper_reflex_200` | ₦200 | consumable | Super-Save Keeper Gloves Pack | +1,200 Golden Balls, 3 Telegraph Save Gloves & 3 Composure Refills |
| `var_rekick_300` | ₦300 | consumable | VAR Second-Chance Pack | +1,800 Golden Balls & 5 VAR Re-Kick Tokens |
| `golden_boot_1000` | ₦1,000 | one_off | Jay-Jay Golden Boot (Permanent) | Permanent +25% Shot Accuracy, 2× Match Coin Rewards forever & 1,000 Golden Balls |
| `super_eagles_2500` | ₦2,500 | one_off | Super Eagles Captain Pass | All stadiums, Golden Keeper Kit, 2× leaderboard score & 3,000 Golden Balls |
| `odogwu_club_5000` | ₦5,000 | consumable | Odogwu Club Owner Megacrate | +15,000 Golden Balls, 15 Fireball Boots, 15 Super-Save Gloves & 10 VAR Re-Kick Tokens |

## Submission checklist

- [ ] Obtain Lagos Life's current submission, trademark/branding, game-hub registration, deep-link, and review requirements; this repository cannot certify acceptance.
- [ ] Deploy Render service from `render.yaml`; set both HMAC secrets and Vatar server credentials.
- [ ] Set up persistent disk or external durable storage for production player/claim state.
- [ ] Add the six product IDs and prices exactly as shown above in Vatar dashboard.
- [ ] Register `/vatar-webhook` and test purchase and delivery callbacks.
- [ ] Register the root launch URL in the Games/Stadium hub and verify the hub opens the service in the correct embedded context.
- [ ] Test each `?screen=` deep link and the share-card challenge link from an external messaging app.
- [ ] Test on Lagos Life in-game browser (not only a standalone browser); SDK checkout requires the platform host.
- [ ] Verify account binding and cloud-save restoration across devices using real platform sessions.
- [ ] Confirm the signed result callback is configured and the leaderboard moves from its empty state using actual test settlements.
- [ ] Review approved brand copy and listing images in `public/assets/` (icon-512 and four 750 × 1334 portrait screenshots).
- [ ] Confirm `GET /api/dev-checklist` returns configured webhook/settlement items after secrets are set.
