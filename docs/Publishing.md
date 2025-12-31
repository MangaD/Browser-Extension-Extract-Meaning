Here’s the **exact, no-nonsense checklist** to publish properly on **Firefox Add-ons (AMO)** and **Chrome Web Store**, given your current setup (dual manifests, build script, dark UI, history access).

I’ll keep this practical and linear.

---

# 0️⃣ Final sanity check (5 minutes, do this once)

Before publishing anywhere:

### Code

* ✅ `candidateFromQuery` matches **queries ending in `" meaning"`**
* ✅ Multi-word phrases work (`modus operandi meaning`)
* ✅ Scan finds expected volume of results
* ✅ Clear → Scan → Reload works as expected

### Permissions

If you are **not using** these yet, remove them now:

* ❌ `tabs`
* ❌ `alarms`

Both stores **scrutinize permissions**.
If you keep them, reviewers may ask *why*.

**Recommended permissions list:**

```json
"permissions": ["history", "storage"]
```

---

# 1️⃣ Build the release artifacts

From repo root:

```bash
npm run build:firefox
npm run build:chrome
```

You should now have:

```
dist/
├── firefox/
│   ├── manifest.json   (MV2)
│   ├── src/
│   └── icons/
└── chrome/
    ├── manifest.json   (MV3)
    ├── src/
    └── icons/
```

---

# 2️⃣ Prepare store assets (same for both stores)

You need **once**, reuse everywhere:

### Icons

You already have:

* ✅ 48×48 PNG
* ✅ 128×128 PNG

Optional but recommended:

* 96×96
* 512×512 (for Chrome listing)

---

### Screenshots (required)

Take **at least 2**:

1. **Popup – before scan**

   * Dark UI
   * Engine selector + Scan button
2. **Popup – after scan**

   * Word list + counts + examples

Optional (good):

* Export buttons visible
* Multi-word example shown

---

### Short description (store blurb)

You’ll paste this verbatim:

> Extracts all “<word> meaning” searches from your browser history.
> Supports Google, Bing, and DuckDuckGo.
> Local-only processing. Export results as CSV or JSON.

---

### Long description (use this)

(Stores love clarity + privacy emphasis)

> This extension scans your browser history to find searches that end in “meaning” (for example: “altruism meaning” or “modus operandi meaning”).
>
> It supports Google, Bing, and DuckDuckGo, stores results locally, and allows exporting the extracted words as CSV or JSON.
>
> All processing happens locally in your browser. No data is transmitted or collected.

---

### Privacy policy (yes, even for simple extensions)

Create **PRIVACY.md** (or paste into store field):

```
This extension processes browser history locally to extract search queries
that end in the word "meaning".

No data is transmitted, collected, or shared.
No analytics or tracking are used.
All extracted data is stored locally using browser extension storage
and can be cleared at any time by the user.
```

---

# 3️⃣ Publish to Firefox Add-ons (AMO)

### A) Zip the Firefox build

From `dist/firefox/`:

```bash
zip -r word-meaning-history-firefox.zip .
```

### B) Go to

👉 [https://addons.mozilla.org/developers/](https://addons.mozilla.org/developers/)

### C) Upload

* Choose **“On this site”**
* Upload the ZIP
* Select:

  * Category: **Productivity** or **Developer Tools**
  * Type: **Extension**

### D) Answer reviewer questions

When asked:

* **Why history permission?**
  → “Required to scan search URLs in browsing history.”
* **Data collection?**
  → “None.”

### E) Review time

* Usually **minutes to a few hours** for simple extensions
* If rejected, it’s almost always:

  * unnecessary permissions
  * unclear privacy wording

---

# 4️⃣ Publish to Chrome Web Store

### A) Zip the Chrome build

From `dist/chrome/`:

```bash
zip -r word-meaning-history-chrome.zip .
```

### B) Go to

👉 [https://chrome.google.com/webstore/devconsole](https://chrome.google.com/webstore/devconsole)

### C) One-time fee

* **$5 USD** (only once per developer account)

### D) Upload ZIP

* Category: **Productivity**
* Visibility: Public (or Unlisted first)

### E) Permissions justification

Chrome is strict. For `history`, explain:

> This permission is required to read search URLs from browser history in order to extract queries ending in the word “meaning”.

---

# 5️⃣ Versioning & future updates

You’re good with:

```json
"version": "0.1.0"
```

Next updates:

* Bugfix → `0.1.1`
* UI improvement → `0.2.0`
* Feature (date filtering, stats) → `0.3.0`

Keep versions **identical** across both manifests.

---

# 6️⃣ GitHub release (recommended)

Tag a release:

```bash
git tag v0.1.0
git push origin v0.1.0
```

Create a GitHub Release:

* Title: `v0.1.0 – Initial release`
* Attach:

  * Firefox ZIP
  * Chrome ZIP
* Mention:

  * Multi-word support
  * Local-only privacy
  * Firefox + Chrome support

---

# 7️⃣ Portfolio update (you should do this)

On your site / JSON:

* Mark as **Published**
* Add store links once approved
* Mention:

  * Browser APIs
  * Privacy-first design
  * Dual MV2/MV3 strategy

This is *very* attractive for reviewers/employers.

---

## TL;DR checklist

✅ Build both targets
✅ Remove unused permissions
✅ Prepare screenshots + privacy text
✅ Zip & upload Firefox build
✅ Zip & upload Chrome build
✅ Create GitHub release

If you want, next I can:

* write **exact AMO answers** (copy/paste)
* review your **Chrome Web Store listing text**
* or help you **deprecate MV2 cleanly later** when Firefox fully enables MV3

Just say the word.

---

Publishing to **Microsoft Edge Add-ons** is thankfully the *easiest* of the three. Since you already have a **Chrome MV3 build**, Edge will accept it with **no code changes**.

Below is the **exact step-by-step**, tuned to your project.

---

# Publish on Microsoft Edge Add-ons (step-by-step)

## 1️⃣ Use your **Chrome build**

Edge is Chromium-based.

👉 Use **the same ZIP you built for Chrome**
No Edge-specific manifest needed.

```bash
npm run build:chrome
```

This gives you:

```
dist/chrome/
└── extract-meaning-words-chrome-v0.1.0.zip
```

That ZIP is what you upload to Edge.

---

## 2️⃣ Create a Microsoft Partner account

Go to:

👉 [https://partner.microsoft.com/dashboard/microsoftedge](https://partner.microsoft.com/dashboard/microsoftedge)

* Sign in with a Microsoft account
* Accept the Edge Add-ons developer agreement
* **No fee** (unlike Chrome)

---

## 3️⃣ Submit the extension

Click **“New extension”** → upload:

```
extract-meaning-words-chrome-v0.1.0.zip
```

Edge will automatically:

* Detect MV3
* Parse permissions
* Validate the manifest

---

## 4️⃣ Fill out the listing (important fields)

### Name

Use the **same name** as Chrome/Firefox for consistency, e.g.:

> **Word Meaning History**

(Strongly recommended.)

---

### Short description

(max ~150 chars)

> Extracts all “<word> meaning” searches from your browser history.
> Supports Google, Bing, and DuckDuckGo.

---

### Long description

You can reuse the Chrome one verbatim:

> This extension scans your browser history to extract searches that end in “meaning”, such as “altruism meaning” or “modus operandi meaning”.
>
> It supports Google, Bing, and DuckDuckGo and allows exporting the extracted words as CSV or JSON.
>
> All processing happens locally. No data is transmitted or collected.

---

### Category

Choose one:

* **Productivity**
* **Developer Tools**

(Productivity is usually safer.)

---

### Screenshots (required)

Same screenshots as Chrome:

* Popup before scan
* Popup after scan

---

## 5️⃣ Permissions explanation (very important)

Edge *will* flag `history`.

When asked “Why does your extension need this permission?”, use this exact wording:

> The extension needs access to browsing history in order to read search result URLs and extract queries ending in the word “meaning”.
> All processing happens locally, and no data is transmitted or collected.

This satisfies Edge reviewers.

---

## 6️⃣ Privacy section

Edge is strict but simple.

Set:

* **Collects personal data?** → ❌ No
* **Shares data with third parties?** → ❌ No
* **Data sold?** → ❌ No

If there’s a free-text field, paste your privacy policy text.

---

## 7️⃣ Submit for review

Click **Submit**.

### Review time

* Usually **a few hours**
* Sometimes **same day**
* Much faster than Chrome, generally smoother than Firefox

You’ll get an email when it’s live.

---

# Optional but recommended tweaks for Edge

### A) Add a `short_name` (nice polish)

Edge sometimes shows this in compact UI.

Add to **Chrome manifest only**:

```json
"short_name": "Word Meaning"
```

Not required, but nice.

---

### B) Remove unused permissions

If you’re not using them yet, remove:

```json
"tabs",
"alarms"
```

Edge reviewers like minimal permission sets.

---

## 8️⃣ Final cross-store compatibility summary

| Store            | Manifest | Build used    | Notes                               |
| ---------------- | -------- | ------------- | ----------------------------------- |
| Firefox AMO      | MV2      | Firefox build | Needs `data_collection_permissions` |
| Chrome Web Store | MV3      | Chrome build  | $5 fee                              |
| Edge Add-ons     | MV3      | Chrome build  | Free, easiest                       |

---

## TL;DR (Edge)

1. `npm run build:chrome`
2. Upload the Chrome ZIP to Edge Add-ons
3. Explain `history` permission clearly
4. Submit → wait a few hours

---

If you want, next I can:

* review your **Edge listing text** line-by-line,
* help you **sync versions** across all stores,
* or plan the **MV2 → MV3 Firefox migration** cleanly for later.

Just say what’s next.

---

# Publish on Brave

Publishing on **Brave** is refreshingly simple — because **you don’t publish separately**.

## The short answer

👉 **If your extension is approved on the Chrome Web Store, it is automatically available to Brave users.**

There is **no Brave Add-ons store** and **no extra submission step**.

---

## Why this works

**Brave** is Chromium-based and uses the **Chrome Web Store** as its extension source.

By default:

* Brave can install extensions directly from the Chrome Web Store
* Brave users see your extension exactly as Chrome users do
* Updates are pulled automatically

---

## What you need to do (checklist)

### ✅ 1. Publish to Chrome Web Store

You already have this covered:

* MV3 manifest
* ZIP uploaded
* Approved listing

Once it’s live there, **Brave is done automatically**.

---

### ✅ 2. Verify Brave compatibility (local test — optional but recommended)

1. Open Brave
2. Go to:

   ```
   brave://extensions
   ```
3. Enable **Developer mode**
4. Click **Load unpacked**
5. Select your `dist/chrome/` folder

Check:

* Popup loads
* Scan works
* History permission prompt appears correctly
* Export works

If it works in Chrome, it will work in Brave.

---

## What Brave users will see

* Your **Chrome Web Store listing**
* Same name, description, screenshots
* Same permissions warning
* Same updates

You do **not** need:

* a Brave account
* a Brave-specific manifest
* a Brave privacy declaration

---

## Optional: mention Brave explicitly (nice polish)

You *can* add this line to your README or store description:

> Compatible with Chrome, Edge, and Brave browsers.

That’s accurate and helpful.

---

## Important caveats (worth knowing)

### 🔹 Brave Shields

Brave’s privacy features **do not block**:

* `chrome.history`
* extension local storage
* browser UI scripts

Your extension is **not affected** by Shields.

---

### 🔹 Brave Rewards / Ads

Not relevant unless:

* you inject content scripts
* you interact with ads
  You don’t — so nothing to worry about.

---

## TL;DR

| Browser | Action needed               |
| ------- | --------------------------- |
| Chrome  | Publish on Chrome Web Store |
| Edge    | Upload same Chrome ZIP      |
| Brave   | **Nothing extra**           |
| Firefox | Separate AMO submission     |

Once Chrome approves your extension, **Brave users can install it immediately**.

If you want, I can:

* double-check your Chrome listing text for Brave friendliness,
* suggest a **one-line Brave compatibility note** for your portfolio,
* or help you test Brave-specific edge cases (there are very few).

Just say the word.
