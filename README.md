# SiteScout

Construction-site lead app. Scouts report sites (photo + exact GPS, one report per site) and earn a flat ₹150 per verified lead. Suppliers prepay a wallet and unlock leads for their trade and the site's stage.

Node 22, Express, built-in SQLite. No build step.

## Run on your computer
    npm install
    npm run dev        # http://localhost:3000, admin phone 9000000001
    npm test           # 17 API tests
    node test/e2e.mjs  # browser walk-through (needs Chromium; set CHROME=...)

## Put it online (Render)
1. Put this folder in a GitHub repository.
2. Render → New → Blueprint → pick the repo. It reads `render.yaml`.
3. When asked, enter `ADMIN_PHONES` (your 10-digit mobile).
4. After deploy, open Environment and copy `ADMIN_PASSCODE`.
5. Open the link on your phone, sign in with your number, tap "Admin sign-in" and enter the passcode. Then use Add to Home Screen.

The database lives on the disk mounted at `/data`. Free hosts wipe their disk on restart, so use a plan with a persistent disk, or the data will be lost.

## Settings (environment variables)
| Name | Meaning |
|---|---|
| ADMIN_PHONES | Comma-separated phones that become admin |
| ADMIN_PASSCODE | Extra secret admins type at sign-in. Required in production while OTP_MODE=demo |
| OTP_MODE | `demo` shows the login code on screen |
| PAYMENTS_MODE | `demo` lets suppliers add free credit |
| ALLOW_MANUAL_GPS | `true` allows typed coordinates (testing only) |
| DATA_DIR | Where the database and photos are kept |

## Pilot limits (not production-ready)
With `OTP_MODE=demo`, anyone can sign in as any non-admin number. Use it for a small trusted pilot only.

## Still to build before a public launch
- Real SMS login code (MSG91, Twilio or similar) and `OTP_MODE` for it
- Real payments for supplier top-ups (Razorpay) and UPI payouts to scouts; ask a lawyer whether Play Billing applies
- Privacy policy and consent screen (India DPDP Act): owner phone numbers are personal data
- Fake-GPS detection (needs a native wrapper), selfie/face-free proof of presence
- Tamil language
- Backups of `/data`, and a move to Postgres if traffic grows

## Play Store
Wrap the live site as a Trusted Web Activity with Bubblewrap (`npx @bubblewrap/cli init --manifest https://YOUR-SITE/manifest.webmanifest`), build the AAB, and upload. Needs a $25 developer account; a new personal account must run a closed test with 12+ testers for 14 days first.
