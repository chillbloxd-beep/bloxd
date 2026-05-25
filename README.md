# Bloxd Merchant OS

A nice-looking Bloxd SMP trading dashboard for tracking:

- Buy from Orders
- Buy from Market
- Sell to Orders
- Sell on Market
- Recent transactions
- Block/item profit history
- Inventory
- Searchable Bloxd block database
- GitHub Pages frontend
- Cloudflare Worker API
- Cloudflare D1 SQL database

## Folder structure

```txt
frontend/   Static frontend for GitHub Pages
worker/     Cloudflare Worker API + D1 migration
```

## Deploy backend

```bash
cd worker
npm install
npx wrangler login
npx wrangler d1 create bloxd_merchant_os
```

Paste the returned `database_id` into `worker/wrangler.toml`.

Then run:

```bash
npx wrangler d1 migrations apply bloxd_merchant_os --remote
npx wrangler deploy
```

Copy the deployed Worker URL.

## Deploy frontend to GitHub Pages

Upload the contents of `frontend/` to your GitHub repo.

In GitHub:

```txt
Settings → Pages → Deploy from branch → main → /root
```

Open the GitHub Pages URL, paste your Worker API URL at the top, and click Save.

## Seed your 1120 Bloxd blocks

In the app:

```txt
Settings → Paste full block list → Import Blocks to D1
```

The parser will turn lines like:

```txt
Maple Door [O,R]
Wheat [FG]
Stone
```

into:

```txt
name: Maple Door
meta_codes: O,R
category: Door / Trapdoor
```

## API routes

```txt
GET  /api/health
GET  /api/items
GET  /api/items?search=diamond
POST /api/items/import
GET  /api/trades
POST /api/trades
GET  /api/stats/summary
GET  /api/inventory
GET  /api/history/blocks
```

## Important

Do not put Cloudflare API keys in the frontend. The frontend calls the Worker. The Worker talks to D1 through the DB binding.


## Login routes and cookie auth

Frontend routes:

```txt
https://chillbloxd-beep.github.io/bloxd-merchant-os/login
https://chillbloxd-beep.github.io/bloxd-merchant-os/dashboard
```

Set Worker secrets before login works:

```bash
cd worker
npx wrangler secret put LOGIN_USERNAME
npx wrangler secret put LOGIN_PASSWORD
npx wrangler secret put SESSION_SECRET
npx wrangler deploy
```

The Worker issues an HTTP-only cookie named `bmo_session`. API endpoints require this cookie.

Because GitHub Pages is static, the zip includes `frontend/login/index.html` and `frontend/dashboard/index.html` so those routes do not 404.
