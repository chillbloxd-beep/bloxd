# Codespaces setup for login + cookies

1. Open your repo in Codespaces.

```txt
Code → Codespaces → Create codespace on main
```

2. Replace/upload the updated `frontend/` and `worker/` folders from this zip.

3. Install Wrangler dependencies.

```bash
cd worker
npm install
```

4. Login to Cloudflare.

```bash
npx wrangler login
```

5. Create D1 if you have not done it yet.

```bash
npx wrangler d1 create bloxd_merchant_os
```

Paste the returned `database_id` into `worker/wrangler.toml`.

6. Apply migrations.

```bash
npx wrangler d1 migrations apply bloxd_merchant_os --remote
```

7. Set login secrets.

```bash
npx wrangler secret put LOGIN_USERNAME
npx wrangler secret put LOGIN_PASSWORD
npx wrangler secret put SESSION_SECRET
```

Example:

```txt
LOGIN_USERNAME = merchant
LOGIN_PASSWORD = your private password
SESSION_SECRET = random long string
```

8. Deploy Worker.

```bash
npx wrangler deploy
```

9. Commit and push.

```bash
cd ..
git add .
git commit -m "Add login routes and cookie auth"
git push
```

10. In GitHub Pages, use `/frontend` as the Pages folder.

Your routes:

```txt
/bloxd-merchant-os/login
/bloxd-merchant-os/dashboard
```
