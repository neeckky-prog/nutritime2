# NutriTime — Deploy & Test Checklist

Live site: **https://nutritime2.vercel.app**

---

## 1. One-time prep (do this once)

### a) Generate the app icons
1. Open `_make-icons.html` in your browser (double-click it).
2. Click **both** download buttons.
3. Save the two files **into this same `nutritime` folder**, keeping the exact names:
   - `icon-192.png`
   - `icon-512.png`

> The `icon.svg` already works for Android/Chrome installs. The PNGs are for iPhone's home-screen icon.

### b) Get a free Gemini API key (powers the AI food scan)
1. Go to https://aistudio.google.com
2. Click **Get API key** → copy it.

---

## 2. Deploy to Vercel

1. Push this `nutritime` folder to Vercel (drag-and-drop, or via GitHub import).
2. In Vercel → your project → **Settings → Environment Variables**, add:

   | Name | Value |
   |------|-------|
   | `GEMINI_API_KEY` | *(your key from step 1b)* |
   | `ALLOWED_ORIGIN` | `https://nutritime2.vercel.app` |

   > `ALLOWED_ORIGIN` stops other websites from spending your Gemini quota.
   > Use the exact URL — no trailing slash.

3. **Redeploy** (Deployments → ⋯ → Redeploy). Environment variable changes only apply after a redeploy.

---

## 3. Test on your iPhone (in order)

1. Open **https://nutritime2.vercel.app** in **Safari** (NOT Chrome — iOS camera only works in Safari).
2. Tap **📷 Enable Camera** → tap **Allow**.
3. **Barcode test:** tap **🔖** → point at any packaged-food barcode → it should buzz and fill in the nutrition.
4. **AI food test:** tap the round **shutter** → take a photo of a meal → wait for the AI result.
5. **Supplement test:** the default cabinet has an Iron Tablet. Scan a high-calcium meal (milk, cheese) → you should see *"Wait 2 hours before your iron tablet"* with a reminder button.
6. **Install:** tap **Share → Add to Home Screen** → confirm the icon appears.

---

## 4. After you change the code and redeploy

The app caches itself to work offline, so phones may show the **old version** first.
To force the new version:
- Pull down to refresh **twice**, OR
- Remove the app from the Home Screen and re-add it.

(New visitors get the latest automatically — the cache version was bumped to `v4`.)

---

## What works where

| Feature | Needs |
|---------|-------|
| AI food photo scan | HTTPS (Vercel) + `GEMINI_API_KEY` + Safari camera |
| Barcode scan (camera) | Safari camera. Android/Chrome uses fast native scanner; iPhone uses ZXing fallback |
| Barcode by typing the number | Works everywhere, even with no camera (use **＋ Add food**) |
| Demo mode | Works everywhere, no camera/key needed |
| Offline use | Works after first visit (service worker) |

---

## Known limits (next phase, not bugs)

- **2-hour iron reminder** only fires while the app stays open. Reliable background reminders need the native wrapper (Capacitor) — that's the app-store step.
- **No accounts / cloud backup** yet. Data lives on the one device; "Clear all data" or clearing Safari wipes it. Add accounts before charging money.
- **Micronutrient data** (calcium, iron, vitamin C) is often missing for barcoded products — OpenFoodFacts doesn't always have it. Energy, protein, carbs, fat are usually present.
