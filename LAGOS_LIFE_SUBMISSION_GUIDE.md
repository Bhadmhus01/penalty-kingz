# Lagos Life submission — Naija Penalty Kingz

## Dashboard copy (paste-ready)

**Game title:** Naija Penalty Kingz: 1v1 Super Striker & Keeper Showdown  
**Platform:** Lagos Life (`https://lagoslife.app`)  
**Short description:** Take the spot kick. Own the Lagos night. Five fast rounds of penalty drama, Nigerian stadiums, skill upgrades and matchday rewards.  
**Full description:** Step into the boots of a Lagos legend in a mobile-first 1v1 penalty showdown. Pick your shot style, read the keeper, bend the ball into a corner, then dive to deny your rival. Play Ajegunle, Onikan, Uyo and Abuja, upgrade four player attributes, and chase the Matchday leaderboard. Includes responsive Canvas gameplay, local progress recovery, Vatar Games SDK v1 contextual purchase integration, and a Nigerian matchday soundscape.  
**Category:** Sports / Football  
**Orientation:** Portrait; optimized for 375 × 812 and responsive mobile screens  
**Launch URL:** `https://<your-render-service>.onrender.com/`  
**Allowed embedding origins:** `https://lagoslife.app`, `https://www.lagoslife.app`, `https://naijalife.app`, `https://www.naijalife.app`, `https://lagoslife.eliysites.com`  
**Health check:** `/healthz`  
**Developer checklist:** `/api/dev-checklist`  
**Suggested tags:** Nigeria, Lagos, football, penalties, sports, casual, skill, Super Eagles

## Vatar SDK & server setup

The game loads `https://lagoslife.app/sdk/vatar-games-v1.js`, calls `Vatar.purchase(productId)` at purchase entry points, and listens for `vatar:purchase-complete` messages. Configure the Vatar dashboard callback/webhook URL as `https://<your-render-service>.onrender.com/vatar-webhook`; configure the result signing secret as `VATAR_RESULTS_SECRET`. Set `VATAR_WEBHOOK_SECRET` and `VATAR_RESULTS_SECRET` in Render before publishing. If Vatar provides a server verification API, set `VATAR_API_URL` and `VATAR_SERVER_TOKEN`; never expose the server token in client code. Session endpoint: `POST /session/verify`. Purchase fulfillment: `POST /claim` (also `/api/claim`). Signed result endpoint: `POST /api/match/settle`.

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

- [ ] Deploy Render service from `render.yaml`; set both HMAC secrets and Vatar server credentials.
- [ ] Set up persistent disk or external durable storage for production player/claim state.
- [ ] Add the six product IDs and prices exactly as shown above in Vatar dashboard.
- [ ] Register `/vatar-webhook` and test purchase and delivery callbacks.
- [ ] Test on Lagos Life in-game browser (not only a standalone browser); SDK checkout requires the platform host.
- [ ] Review listing images in `public/assets/` (icon-512 and four 750 × 1334 portrait screenshots).
- [ ] Confirm `GET /api/dev-checklist` returns configured webhook/settlement items after secrets are set.
