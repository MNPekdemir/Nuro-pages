# Dhikr Flow — Store Release Guide

Everything needed to publish to Google Play (and later the App Store). Items marked
**⚠️ YOU** require your account/action and cannot be done from code.

## 1. Signing keystore ✅ (generated)
A real **upload keystore** was generated and wired into the release build:

- Keystore: `android/upload-keystore.jks`  (alias `upload`, RSA 2048, valid ~27 years)
- Config: `android/key.properties` (git-ignored)
- **Password (store & key):** `DhikrFlow-2026-Zikir!`

**⚠️ YOU — critical, do this now:**
1. **Back up `android/upload-keystore.jks` AND the password** somewhere safe (password manager +
   offline copy). If you lose either, you can **never update the app** under the same listing.
2. `android/key.properties` and `*.jks` are git-ignored so they won't be committed.
3. (Optional) change the password to your own:
   `keytool -storepasswd -keystore android/upload-keystore.jks` then update `key.properties`.
4. Consider enrolling in **Google Play App Signing** (Play holds the app-signing key; you keep this
   upload key). Recommended.

The release build now signs with this key automatically (falls back to debug only if
`key.properties` is absent).

## 2. Build artifacts
```
flutter build appbundle --release   # -> build/app/outputs/bundle/release/app-release.aab  (upload this)
flutter build apk --release         # -> build/app/outputs/flutter-apk/app-release.apk      (sideload/test)
```
`versionName`/`versionCode` come from pubspec `version: 1.0.0+1`. Bump the `+N` for every upload.

## 3. In-app purchases (Support the app) ⚠️ YOU
Feature is coded and ready; the products must exist in **Play Console → Monetize → In-app products**.
Create these exact product IDs (non-consumable, "Supporter" tier). Prices are suggestions:

| Product ID          | Suggested price |
|---------------------|-----------------|
| `support_small`     | ₺19,99          |
| `support_medium`    | ₺59,99          |
| `support_large`     | ₺119,99         |
| `support_generous`  | ₺249,99         |

They **never unlock features** (ethical / by design). Test with a Play *license tester* account.

## 4. Privacy policy & terms ⚠️ YOU (host these)
- In-app copies live under Settings → About.
- Hostable pages for the **store listing** are in `store/privacy.html` and `store/terms.html`.
- Host them (e.g. free **GitHub Pages**) and paste the **Privacy policy URL** into
  Play Console → App content → Privacy policy. A URL is **required** to publish.

## 5. Support contact
`AppConstants.supportEmail` is `pekdemir.mn@gmail.com`. Swap it for a dedicated support address if
you'd rather not expose your personal inbox.

## 6. Play Console "App content" checklist ⚠️ YOU
- Privacy policy URL (see §4)
- **Data safety** form: declare *no data collected / no data shared* (app is local-only).
- Ads: **No ads**.
- Content rating questionnaire (Everyone).
- Target audience, and a note that names/meanings are reference-only (not fatwa) if asked.

## 7. Store listing assets ⚠️ YOU
Icon is set (`assets/icon/`). Still need: feature graphic (1024×500), 2–8 phone screenshots,
short + full description (tr + en; other locales optional — the app itself is fully localized in
tr/en/de/fr/id/ar).

## Status
Code side is production-ready: `flutter analyze` clean, full test suite green, release build signs
with the real keystore, all 6 locales fully translated, crash-hardened against corrupt storage.
Remaining is account/console work above.
