# Fortify Health – Premix Batch Scanner

Mobile-first web app (PWA) for Quality Officers and field staff at partner wheat flour mills.

**Scan → Verify → Enter Quantity → Submit**

## Features
- Live camera scanning of QR codes and barcodes (rear camera, flashlight toggle where supported)
- Photo-of-label fallback and manual batch selection for damaged labels
- Locked, system-generated batch details, with a flagged "Edit Batch Details" mode
- Quantity entry in kg or packs, with quick buttons (25 / 50 / 100 / 225 kg)
- Transaction types: Receive, Consume, Relocate (with destination mill), Adjust (with reason)
- Automatic validation: registered batch, expiry and near-expiry (≤120 days), stock limit, duplicates, required fields
- Expired batches are blocked unless an authorised override is recorded
- Offline mode: transactions are queued on the device and synced when the connection returns
- Batch-wise inventory with search and filters (All, Valid, Near Expiry, Expired, Low Stock)
- Transaction log with unique IDs (PMX-YYYY-NNNNNN) and timestamps

## Files
| File | Purpose |
| --- | --- |
| `index.html` | The whole app (HTML, CSS and JavaScript) |
| `manifest.webmanifest` | Lets phones install the app on the home screen |
| `sw.js` | Service worker that caches the app for offline use |
| `icons/` | App icons |
| `.nojekyll` | Tells GitHub Pages to serve files as they are |

## Publish with GitHub Pages
1. Create a new repository, for example `premix-batch-scanner`.
2. Upload every file in this folder to the root of the repository (keep the `icons` folder).
3. Go to **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, then branch `main` and folder `/ (root)`. Save.
4. After a minute or two, the app is live at `https://<your-username>.github.io/premix-batch-scanner/`.

The camera needs https, which GitHub Pages provides. When the browser asks for camera permission, tap **Allow**.

To install on Android: open the link in Chrome, then tap **⋮ → Add to Home screen**. On iPhone: open in Safari, then tap **Share → Add to Home Screen**.

## Sample data
Batches, mills, stock and transactions are sample data held in `index.html` (search for `BATCHES`, `MILLS` and `seed()`). Entries are stored only in each phone's browser (localStorage). Use **Profile → Sample Data → Reset** to start again.

## Label format
The scanner accepts any of these label contents:
- `FH-PMX;BATCH=AHD26098;VENDOR=Hexagon Nutrition;MFG=2026-01-23;EXP=2027-01-22;PACK=25`
- JSON: `{"batch":"AHD26098","vendor":"Hexagon Nutrition","mfg":"2026-01-23","exp":"2027-01-22","pack":25}`
- A plain batch number, for example `AHD26098`

## Next steps for production
- Replace the sample `BATCHES` and `MILLS` with API calls to the Premix Batch Master.
- Send transactions to the Transaction Table API instead of localStorage (the record already carries ID, timestamp, user, mill, batch, type, quantity, source, destination, remarks, GPS and sync status).
- Add sign-in so the user and mill come from the logged-in account.
- Bump `VERSION` in `sw.js` whenever you change `index.html`, so phones pick up the new version.
