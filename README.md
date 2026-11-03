# Döner Kebab Simulator for Android

A 3D kebab shop game. Shave meat off the spit, fill the bread to match the customer's ticket, earn tips, buy upgrades and work your way through the days. Free to play, with a one-time **Full shop** unlock sold through Google Play.

- The game itself is `www/index.html` (runs fully offline).
- The `android` folder is the Android Studio project that wraps it as an app (Capacitor).
- `.github/workflows/build-apk.yml` builds the app on GitHub.

---

## What's in the game

**Free**
- Career mode: one day after another, more customers each day, a lunch rush every 4th day
- Customer types: regulars, people in a hurry, kid portions, gym bros and picky eaters
- Upgrade shop paid with tips: sharper blade, hotter grill, big scoop, precision bottles, ticket printer, shop radio, tip jar
- 18 achievements, a first-day tutorial, practice mode, saved progress

**Full shop (one-time purchase, product ID `full_shop`)**
- Dürüm wraps, Kebabtallerken with fries, falafel orders
- Chicken spit next to the beef
- Six more ingredients: fries, feta, jalapeño, corn, falafel, kebab dressing
- Night shift with after-party customers and 30% bigger tips
- Three extra kitchen looks

The purchase only works in the Android app installed from Google Play. The browser version has a free "Try the full shop" preview instead.

---

## Build the APK (for testing and sharing)

### On GitHub
1. Upload everything in this folder **except `node_modules`** to your repository. If you already uploaded an earlier version, upload again on top; matching files are replaced.
   Check that `.github/workflows/build-apk.yml` is in the repo. If the hidden `.github` folder doesn't upload, create the file on GitHub with *Add file → Create new file* and paste in the contents.
2. Open **Actions**. *Build Android APK* runs on every upload (about 5 minutes).
3. Open the finished run and download **doner-kebab-simulator-apk** under *Artifacts*.

### With Android Studio
Open the `android` folder with *File → Open*, wait for Gradle sync, then *Build → Build App Bundle(s) / APK(s) → Build APK(s)*.

Note: the in-app purchase does not work in a sideloaded APK. You can test it once the app is on a Google Play test track (see below).

---

## Publish on Google Play

### 1. Create a signing key (one time)
Google Play needs a signed App Bundle (`.aab`). In Android Studio: *Build → Generate Signed App Bundle / APK → Android App Bundle → Create new…*. Save the `.jks` file and write down the passwords and the key alias.
**Keep this file and the passwords safe.** You need them for every future update.

### 2. Let GitHub build the signed bundle
In your repository go to *Settings → Secrets and variables → Actions → New repository secret* and add four secrets:

| Secret | Value |
|---|---|
| `KEYSTORE_BASE64` | The `.jks` file encoded as text. Windows PowerShell: `[Convert]::ToBase64String([IO.File]::ReadAllBytes("C:\path\upload.jks"))`. Mac/Linux: `base64 -i upload.jks` |
| `KEYSTORE_PASSWORD` | The keystore password |
| `KEY_ALIAS` | The key alias (often `key0` or `upload`) |
| `KEY_PASSWORD` | The key password |

Then run the workflow again (*Actions → Build Android APK → Run workflow*). The run now also produces **doner-kebab-simulator-aab**. Every run gets a higher version number automatically, which Google Play requires for each upload.

Alternatively build it in Android Studio with *Generate Signed App Bundle*, but then raise `versionCode` in `android/app/build.gradle` yourself before every upload.

### 3. Set up the app in Play Console
1. Create the app at https://play.google.com/console (personal account, one-time USD 25 fee).
2. Before the first upload, decide on the app ID. It is `app.donerkebab.simulator` now and can never change after publishing. To change it, edit `appId` in `capacitor.config.json` and `applicationId`/`namespace` in `android/app/build.gradle`, and move `MainActivity.java` to the matching folder.
3. Upload the `.aab` to **Testing → Closed testing**.
4. Fill in the store listing, the content rating, *Data safety* (the game collects no data) and a privacy policy link.

### 4. Create the in-app product
1. In Play Console: **Monetize with Play → Products → One-time products → Create product**.
2. Product ID: **`full_shop`** (exactly this, the game looks for it).
3. Name: *Full shop*. Description: *Unlocks dürüm, kebabtallerken, falafel, the chicken spit, six ingredients, night shift and kitchen looks.*
4. Set the price (for example NOK 25) and **Activate** the product.
   Products can only be created after a build with billing has been uploaded to a test track.

### 5. Test the purchase
1. Add your own Google account under **Settings → License testing**. Test purchases on that account are free.
2. Install the app from the closed-test link on your phone and tap *Full shop → Unlock*.
3. *Restore purchase* brings the unlock back after reinstalling.

### 6. Go live
New personal developer accounts must run the closed test with at least 12 testers for 14 days in a row. Then click **Apply for production** on the dashboard.

---

## Changing the game

1. Edit `www/index.html`.
2. With Node.js installed, run in this folder:
   ```
   npm install
   npx cap sync android
   ```
3. Build again (GitHub or Android Studio).
